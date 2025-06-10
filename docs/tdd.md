# Technical Design Document (TDD)
## Nia Match - Muslim Marriage Platform MVP

### Technical Architecture Overview

The platform follows a modern, serverless architecture leveraging Next.js 15 App Router for full-stack development, with external services handling specialized functionality like authentication, database management, payments, and communications.

### Core Technology Stack

#### Frontend Framework
**Next.js 15 (App Router)**
- **Rationale**: Server-side rendering for SEO, built-in API routes, excellent TypeScript support
- **Implementation**: React Server Components for performance, client components only when necessary
- **Routing**: File-based routing with layouts for consistent UI patterns

**TypeScript**
- **Rationale**: Type safety, better developer experience, reduced runtime errors
- **Implementation**: Strict mode enabled, interface-driven development

**Tailwind CSS**
- **Rationale**: Utility-first CSS, responsive design, consistent design system
- **Implementation**: Custom design tokens, mobile-first approach

#### Authentication & User Management
**Clerk**
- **Service**: User authentication, session management, 2FA
- **Implementation**: 
  - Custom sign-up flow integrated with Enneagram test
  - Role-based permissions (User vs Wali access)
  - Webhook integration for user lifecycle events
- **API Integration**: Clerk webhooks → Supabase user profile creation

#### Database & ORM
**Supabase (PostgreSQL)**
- **Service**: Primary database, real-time subscriptions, Row-Level Security
- **Implementation**:
  - GDPR-compliant data storage in EU regions
  - Automatic backups and point-in-time recovery
  - Connection pooling for performance

**Drizzle ORM**
- **Library**: Type-safe database queries and migrations
- **Implementation**:
  - Schema-first approach with TypeScript types
  - Migration management through version control
  - Query optimization for matching algorithm

#### Payment Processing
**Stripe**
- **Service**: Subscription billing, payment processing, invoice management
- **Implementation**:
  - Customer Portal for subscription management
  - Webhook handling for subscription status changes
  - Multi-currency support for global users
- **Integration**: Stripe webhooks → Supabase subscription status updates

#### Communication Services
**Plunk**
- **Service**: Email notifications and alerts (not for chat communication)
- **Implementation**:
  - Template-based emails for match notifications
  - Trial expiration and subscription reminders
  - New chat creation notifications to participants
  - Delivery tracking and bounce handling
- **Integration**: Notification-only emails with Islamic etiquette guidelines

### Third-Party Integrations

#### Enneagram Assessment
**Official Enneagram Institute Test**
- **Integration**: Licensed test content via API or embedded iframe
- **Implementation**: 
  - Progress tracking through multi-step form
  - Results processing and personality type calculation
  - Strengths/weaknesses generation based on type
- **Fallback**: Custom implementation if licensing unavailable

#### Analytics & Monitoring
**Vercel Analytics**
- **Service**: Privacy-friendly web analytics
- **Implementation**: User journey tracking, conversion funnel analysis

**Sentry**
- **Service**: Error monitoring and performance tracking
- **Implementation**: Real-time error alerts, performance bottleneck identification

### Core Feature Implementation

#### 1. Matching Algorithm
**Technology**: Custom Node.js algorithm with PostgreSQL queries
**Implementation**:
```typescript
// Enneagram compatibility matrix
const COMPATIBILITY_MATRIX = {
  1: [2, 7, 9], // Type 1 compatible with Types 2, 7, 9
  2: [1, 4, 8], // etc.
  // ... full matrix
};

// Deal-breaker filtering function
async function findMatches(userId: string) {
  const user = await getUserProfile(userId);
  const compatibleTypes = COMPATIBILITY_MATRIX[user.enneagramType];
  
  return await db
    .select()
    .from(users)
    .where(
      and(
        inArray(users.enneagramType, compatibleTypes),
        dealBreakerFilter(user.dealBreakers),
        not(eq(users.id, userId))
      )
    )
    .limit(3);
}
```

#### 2. Real-Time Communication System
**Technology**: WebSocket integration with Next.js and Supabase real-time
**Implementation**:
```typescript
// Group chat creation
async function createGroupChat(match: Match) {
  const chatRoom = await db.insert(chatRooms).values({
    matchId: match.id,
    participants: [
      { userId: match.user1.id, role: 'seeker' },
      { userId: match.user1.waliId, role: 'wali' },
      { userId: match.user2.id, role: 'seeker' },
      { userId: match.user2.waliId, role: 'wali' }
    ],
    status: 'active'
  }).returning();
  
  // Send email notifications about new chat creation
  await notifyParticipants(chatRoom.participants, 'new_chat_created');
  
  return chatRoom;
}
```

#### 3. Subscription Management
**Technology**: Stripe Billing with Supabase integration
**Implementation**:
```typescript
// Trial period tracking
async function checkTrialStatus(userId: string) {
  const user = await getUserWithSubscription(userId);
  const trialEnd = new Date(user.createdAt);
  trialEnd.setDate(trialEnd.getDate() + 14);
  
  return {
    inTrial: new Date() < trialEnd,
    daysRemaining: Math.ceil((trialEnd.getTime() - Date.now()) / (1000 * 60 * 60 * 24))
  };
}
```

### Security & Privacy Implementation

#### Data Protection
**Row-Level Security (RLS)**
- Users can only access their own profile data
- Matches visible only to involved parties and their Walis
- Admin roles for platform monitoring

**Encryption**
- Database encryption at rest (Supabase default)
- TLS encryption for all API communications
- Sensitive data hashing (e.g., preference combinations)

#### GDPR Compliance
**Data Minimization**
- Collect only essential information for matching
- Optional fields clearly marked
- Regular data purging for inactive accounts

**User Rights**
- Data export functionality
- Account deletion with cascade cleanup
- Consent management for communications

### Performance Optimizations

#### Database Optimization
**Indexing Strategy**
- Composite indexes on (enneagram_type, location, age)
- Partial indexes for active users only
- Index on deal_breakers JSONB column for filtering

**Caching**
- Redis caching for match results (if needed at scale)
- CDN caching for static assets
- Browser caching for user preferences

#### Frontend Performance
**Code Splitting**
- Dynamic imports for heavy components (Enneagram test)
- Lazy loading for non-critical features
- Bundle optimization with Webpack

**Image Optimization**
- Next.js Image component for automatic optimization
- WebP format with fallbacks
- Responsive image sizing

### Deployment & Infrastructure

#### Hosting
**Vercel Platform**
- **Benefits**: Seamless Next.js integration, automatic deployments, global CDN
- **Configuration**: 
  - Preview deployments for feature branches
  - Production deployments from main branch
  - Environment variable management

#### Database Hosting
**Supabase Cloud**
- **Configuration**: EU region for GDPR compliance
- **Scaling**: Auto-scaling compute, connection pooling
- **Backup**: Automated daily backups, point-in-time recovery

#### Monitoring & Alerting
**Application Monitoring**
- Vercel Analytics for user behavior
- Sentry for error tracking and performance
- Custom dashboard for business metrics

**Infrastructure Monitoring**
- Supabase built-in monitoring
- Stripe dashboard for payment metrics
- Email notification delivery rate monitoring

### Development Workflow

#### Version Control
**Git Strategy**
- Feature branches for new functionality
- Pull request reviews before merging
- Semantic versioning for releases

#### Testing Strategy
**Unit Testing**
- Jest for business logic testing
- React Testing Library for component testing
- Database query testing with test database

**Integration Testing**
- API endpoint testing
- Payment flow testing (Stripe test mode)
- Email notification testing (Plunk test mode)

#### Deployment Pipeline
1. **Development**: Local development with test databases
2. **Staging**: Preview deployments for testing
3. **Production**: Automated deployment with health checks

### Scalability Considerations

#### Database Scaling
- Connection pooling for concurrent users
- Read replicas for match queries
- Horizontal partitioning by geographic regions

#### Application Scaling
- Vercel serverless functions auto-scale
- CDN caching for global performance
- Background job processing for heavy operations

#### Cost Optimization
- Free tier utilization during MVP phase
- Gradual scaling based on user growth
- Monitoring and alerting for cost thresholds

### Security Audit Checklist

#### Authentication
- ✅ Secure session management (Clerk)
- ✅ Password security standards
- ✅ 2FA implementation
- ✅ Rate limiting on auth endpoints

#### Data Protection
- ✅ Database encryption at rest
- ✅ API encryption in transit
- ✅ Row-level security policies
- ✅ Input validation and sanitization

#### Privacy Compliance
- ✅ GDPR data handling procedures
- ✅ User consent management
- ✅ Data retention policies
- ✅ Right to be forgotten implementation 