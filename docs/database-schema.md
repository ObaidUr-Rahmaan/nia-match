# Database Schema - Nia Match Platform

## Overview
This schema is designed for a subscription-based model with 14-day free trials. Based on the business model discussion, we're implementing a **subscription-only** pricing model, removing one-time payment tables.

## Updated Schema Structure

### Core User Tables

#### 1. Users Table (Updated)
```typescript
import { pgTable, serial, timestamp, varchar, text, integer, jsonb, boolean } from 'drizzle-orm/pg-core';
import { createId } from '@paralleldrive/cuid2';

export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  userId: varchar('user_id').unique().notNull().$defaultFn(() => createId()),
  createdTime: timestamp('created_time').defaultNow(),
  updatedTime: timestamp('updated_time').defaultNow(),
  
  // Basic Info
  email: varchar('email').unique().notNull(),
  firstName: text('first_name'),
  lastName: text('last_name'),
  age: integer('age'),
  gender: varchar('gender'), // 'male' | 'female'
  location: text('location'), // City, Country
  profileImageUrl: text('profile_image_url'), // Optional, may not be used in MVP
  
  // Enneagram Data
  enneagramType: integer('enneagram_type'), // 1-9
  enneagramStrengths: text('enneagram_strengths'),
  enneagramWeaknesses: text('enneagram_weaknesses'),
  enneagramTestCompleted: boolean('enneagram_test_completed').default(false),
  
  // Deal Breakers (JSONB for flexibility)
  dealBreakers: jsonb('deal_breakers'), // Structured JSON object
  
  // Wali Information
  waliName: text('wali_name'),
  waliEmail: varchar('wali_email'),
  waliRelationship: text('wali_relationship'), // father, brother, uncle, etc.
  waliPhone: varchar('wali_phone'),
  
  // Profile Status
  profileCompleted: boolean('profile_completed').default(false),
  isActive: boolean('is_active').default(true),
  lastActiveAt: timestamp('last_active_at').defaultNow(),
  
  // Trial & Subscription
  trialStartDate: timestamp('trial_start_date'),
  trialEndDate: timestamp('trial_end_date'),
  isTrialActive: boolean('is_trial_active').default(true),
  
  // Matching Preferences
  rejectionCount: integer('rejection_count').default(0),
  lastRejectionDate: timestamp('last_rejection_date'),
});

// Indexes for users table
CREATE INDEX idx_users_enneagram_type ON users(enneagram_type);
CREATE INDEX idx_users_location ON users(location);
CREATE INDEX idx_users_age ON users(age);
CREATE INDEX idx_users_gender ON users(gender);
CREATE INDEX idx_users_active ON users(is_active) WHERE is_active = true;
CREATE INDEX idx_users_trial ON users(is_trial_active, trial_end_date);
CREATE INDEX idx_users_deal_breakers ON users USING GIN(deal_breakers);
```

#### 2. Deal Breakers Structure (JSONB Schema)
```typescript
interface DealBreakers {
  aqeedah: string; // 'sunni' | 'shia' | 'sufi' | 'salafi' | 'any'
  locationFlexibility: boolean; // willing to relocate
  maxDistance: number; // in km if location flexibility is false
  livingArrangement: string; // 'with_inlaws' | 'independent' | 'flexible'
  hijrahWillingness: boolean; // willing to migrate for faith
  temperamentPreference: string; // 'patient' | 'energetic' | 'calm' | 'any'
  hygieneImportance: number; // 1-5 scale
  wantsChildren: boolean;
  numberOfChildren: string; // 'none' | '1-2' | '3-4' | '5+' | 'asap'
  socialMediaComfort: string; // 'none' | 'minimal' | 'moderate' | 'active'
  referencesRequired: boolean;
}
```

#### 3. Subscription Plans Table (Updated)
```typescript
export const subscriptionPlans = pgTable('subscription_plans', {
  id: serial('id').primaryKey(),
  createdTime: timestamp('created_time').defaultNow(),
  
  planId: varchar('plan_id').unique().notNull(), // stripe price id
  name: varchar('name').notNull(), // 'Basic Monthly', 'Premium Quarterly'
  description: text('description'),
  
  // Pricing
  price: integer('price').notNull(), // in cents
  currency: varchar('currency').notNull().default('GBP'),
  interval: varchar('interval').notNull(), // 'month' | 'quarter' | 'year'
  intervalCount: integer('interval_count').default(1),
  
  // Features
  maxMatches: integer('max_matches').default(3), // matches per week
  prioritySupport: boolean('priority_support').default(false),
  advancedFilters: boolean('advanced_filters').default(false),
  
  isActive: boolean('is_active').default(true),
});
```

#### 4. Subscriptions Table (Updated)
```typescript
export const subscriptions = pgTable('subscriptions', {
  id: serial('id').primaryKey(),
  createdTime: timestamp('created_time').defaultNow(),
  updatedTime: timestamp('updated_time').defaultNow(),
  
  // Stripe Integration
  subscriptionId: varchar('subscription_id').unique().notNull(),
  stripeCustomerId: varchar('stripe_customer_id').notNull(),
  status: varchar('status').notNull(), // 'active' | 'canceled' | 'past_due' | 'trialing'
  
  // User & Plan
  userId: varchar('user_id').notNull(),
  planId: varchar('plan_id').notNull(),
  
  // Dates
  startDate: timestamp('start_date').notNull(),
  endDate: timestamp('end_date'),
  trialStart: timestamp('trial_start'),
  trialEnd: timestamp('trial_end'),
  
  // Billing
  currentPeriodStart: timestamp('current_period_start'),
  currentPeriodEnd: timestamp('current_period_end'),
  defaultPaymentMethodId: varchar('default_payment_method_id'),
  
  email: varchar('email').notNull(),
});
```

### Matching System Tables

#### 5. Matches Table
```typescript
export const matches = pgTable('matches', {
  id: serial('id').primaryKey(),
  matchId: varchar('match_id').unique().notNull().$defaultFn(() => createId()),
  createdTime: timestamp('created_time').defaultNow(),
  updatedTime: timestamp('updated_time').defaultNow(),
  
  // Users involved
  user1Id: varchar('user1_id').notNull(),
  user2Id: varchar('user2_id').notNull(),
  
  // Match Status
  user1Status: varchar('user1_status').default('pending'), // 'pending' | 'accepted' | 'rejected'
  user2Status: varchar('user2_status').default('pending'),
  overallStatus: varchar('overall_status').default('pending'), // 'pending' | 'mutual' | 'expired' | 'rejected'
  
  // Compatibility Scores
  enneagramCompatibility: integer('enneagram_compatibility'), // 1-10 score
  dealBreakerCompatibility: integer('dealbreaker_compatibility'), // 1-10 score
  overallCompatibility: integer('overall_compatibility'),
  
  // Interaction Tracking
  user1ViewedAt: timestamp('user1_viewed_at'),
  user2ViewedAt: timestamp('user2_viewed_at'),
  expiresAt: timestamp('expires_at'), // 7 days from creation
  
  // Communication
  groupChatCreated: boolean('group_chat_created').default(false),
  groupChatId: varchar('group_chat_id'),
});
```

#### 6. Group Chats Table
```typescript
export const groupChats = pgTable('group_chats', {
  id: serial('id').primaryKey(),
  chatId: varchar('chat_id').unique().notNull().$defaultFn(() => createId()),
  createdTime: timestamp('created_time').defaultNow(),
  
  // Match Reference
  matchId: varchar('match_id').notNull(),
  
  // Participants
  user1Id: varchar('user1_id').notNull(),
  user2Id: varchar('user2_id').notNull(),
  wali1Email: varchar('wali1_email').notNull(),
  wali2Email: varchar('wali2_email').notNull(),
  
  // Chat Status
  status: varchar('status').default('active'), // 'active' | 'closed' | 'suspended'
  
  // Email Integration
  plunkConversationId: varchar('plunk_conversation_id'),
  lastEmailSentAt: timestamp('last_email_sent_at'),
  
  // Moderation
  messageCount: integer('message_count').default(0),
  lastMessageAt: timestamp('last_message_at'),
});
```

## Performance Indexes

```sql
-- Users table indexes
CREATE INDEX idx_users_enneagram_type ON users(enneagram_type);
CREATE INDEX idx_users_location ON users(location);
CREATE INDEX idx_users_age ON users(age);
CREATE INDEX idx_users_active ON users(is_active) WHERE is_active = true;
CREATE INDEX idx_users_deal_breakers ON users USING GIN(deal_breakers);

-- Matches table indexes
CREATE INDEX idx_matches_user1 ON matches(user1_id, overall_status);
CREATE INDEX idx_matches_user2 ON matches(user2_id, overall_status);
CREATE UNIQUE INDEX idx_matches_unique_pair ON matches(
  LEAST(user1_id, user2_id), 
  GREATEST(user1_id, user2_id)
);

-- Subscriptions indexes
CREATE INDEX idx_subscriptions_user_id ON subscriptions(user_id);
CREATE INDEX idx_subscriptions_status ON subscriptions(status);
```

## Row-Level Security (RLS) Policies

```sql
-- Users table RLS
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own profile" ON users
  FOR SELECT USING (auth.uid()::text = user_id);

-- Matches table RLS
ALTER TABLE matches ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own matches" ON matches
  FOR SELECT USING (
    auth.uid()::text = user1_id OR 
    auth.uid()::text = user2_id
  );

-- Group chats RLS
ALTER TABLE group_chats ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own group chats" ON group_chats
  FOR SELECT USING (
    auth.uid()::text = user1_id OR 
    auth.uid()::text = user2_id
  );
```

## Foreign Key Relationships

```sql
-- Subscriptions to users
ALTER TABLE subscriptions ADD CONSTRAINT fk_subscriptions_user 
  FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE;

-- Matches to users
ALTER TABLE matches ADD CONSTRAINT fk_matches_user1 
  FOREIGN KEY (user1_id) REFERENCES users(user_id) ON DELETE CASCADE;
ALTER TABLE matches ADD CONSTRAINT fk_matches_user2 
  FOREIGN KEY (user2_id) REFERENCES users(user_id) ON DELETE CASCADE;

-- Group chats to matches
ALTER TABLE group_chats ADD CONSTRAINT fk_group_chats_match 
  FOREIGN KEY (match_id) REFERENCES matches(match_id) ON DELETE CASCADE;
```

This schema supports the subscription-based business model with comprehensive user profiling, intelligent matching, and Islamic-compliant communication features. 