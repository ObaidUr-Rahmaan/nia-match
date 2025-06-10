# User Flows - Nia Match Platform

## Primary User Flows

### 1. New User Registration & Onboarding Flow

**Actor**: Prospective User (Single Muslim seeking marriage)

**Flow Steps**:
1. **Landing Page Visit**
   - User discovers platform through referral/community
   - Reads Islamic values-focused messaging
   - Clicks "Start Your Journey" CTA

2. **Account Creation**
   - Email Signup via Clerk SignIn Component
   - Anonymous option vs. using real name

3. **Multi-Step Profile Creation Form**
   - **Step 1: Basic Information**
     - Name (or choose to stay anonymous)
     - Gender (Your Gender)
     - Age (Your Age)
     - Height (Your Height)
     - Build (Muscular/Thin/Plus-sized/Average)
   
   - **Step 2: Background Information**
     - Nationality (Your Nationality)
     - Ethnicity (Your Ethnicity)
     - Location (Your Location)
     - Dress Code (Your Dress Code)
     - Marital Status (Your Marital Status)
     - Education (Your Education Level)
     - Willingness to relocate (Yes/No)
   
   - **Step 3: Requirements in a Spouse**
     - If they are Male seeking Female:
       - Hijab/Niqab preference
       - Prays 5x a day requirement
       - Other Islamic practice requirements
     - If they are Female seeking Male:
       - Beard preference
       - Modest dressing requirement
       - Employment/career expectations
       - Other Islamic practice requirements
   
   - **Step 4: Deal-breakers Configuration**
     - Aqeedah (theological beliefs) incompatibility
     - Living with in-laws expectations
     - Unwillingness to make Hijrah eventually
     - Short-tempered or impatient personality
     - Poor hygiene standards
     - Not wanting children
     - Active social media presence (Instagram/TikTok)
     - Having no character references
   
   - **Step 5: Character Assessment**
     - References section: Contact numbers/emails of people who can vouch for character
     - Marital dispute resolution question: Free-text response about how they would handle disagreements
     - AI analysis of dispute response to determine agreeableness and conflict resolution style
   
   - **Step 6: Guardian/Contact Information**
     - For Sisters: Wali contact details (name, relationship, phone, email)
     - For Brothers: Own contact details + Father/Mother contact information
   
   - **Step 7: Enneagram Personality Assessment**
     - Introduction to personality-based matching concept
     - Complete official Enneagram test (15-20 minutes)
     - Progress tracker shows completion status
     - Results presentation with strengths/weaknesses

4. **Profile Review & Activation**
   - Complete profile preview with all entered information
   - Terms of service and Islamic guidelines acceptance
   - Profile activation and trial period start (14 days)

**Exit Criteria**: User has complete profile and enters trial period

---

### 2. Weekly Matching & Review Flow

**Actor**: Active User (During trial or subscription period)

**Flow Steps**:
1. **Match Notification**
   - Email notification: "New matches available"
   - Dashboard badge indicates new matches
   - Login to platform

2. **Match Review Dashboard**
   - Up to 3 new matches displayed weekly
   - Each match shows:
     - Enneagram type compatibility
     - Strengths/weaknesses overview
     - Deal-breaker alignment score
     - Compatibility percentage

3. **Individual Match Evaluation**
   - Detailed compatibility breakdown
   - Enneagram relationship dynamics explanation
   - Deal-breaker comparison matrix
   - "Why this match?" AI-generated summary

4. **Decision Making**
   - Three options per match:
     - ✅ Accept: Interested in proceeding
     - ❌ Reject: Not compatible
     - 💾 Save: Review later (expires in 7 days)

5. **Rejection Feedback (If applicable)**
   - After 5 consecutive rejections:
     - Modal appears with statistics
     - Suggestion to review deal-breaker settings
     - Link to preferences modification

**Exit Criteria**: User has reviewed all available matches and made decisions

---

### 3. Mutual Match & Communication Flow

**Actor**: Two Users + Two Walis (4 total participants)

**Flow Steps**:
1. **Mutual Match Detection**
   - System identifies when both users accept each other
   - Automatic match status update to "mutual"
   - Preparation for group chat creation

2. **Group Chat Initiation**
   - System creates secure group chat room
   - All 4 participants (User1, User2, Wali1, Wali2) invited
   - Islamic communication guidelines displayed
   - Introduction template provided

3. **Initial Introductions**
   - Guided conversation starters suggested
   - Family introduction format recommended
   - Islamic etiquette reminders integrated
   - Chat moderation rules displayed

4. **Ongoing Communication**
   - Real-time messaging between all participants
   - Wali oversight ensures Islamic compliance
   - File sharing (documents only, no images)
   - Voice message capabilities (future)

5. **Communication Outcomes**
   - Families decide to proceed with engagement process
   - OR families decide incompatibility and close chat
   - Match status updates accordingly

**Exit Criteria**: Families reach decision on compatibility

---

### 4. Trial Expiration & Subscription Flow

**Actor**: Trial User (Day 12-14 of trial)

**Flow Steps**:
1. **Trial Warning Notifications**
   - Day 12: Email reminder of upcoming expiration
   - Day 13: In-app notification with subscription CTA
   - Day 14: Final warning with payment link

2. **Subscription Decision Point**
   - Value proposition presentation
   - Success stories and testimonials
   - Pricing plans display (monthly/quarterly/annual)
   - Limited-time discount offer (if applicable)

3. **Payment Processing (If subscribing)**
   - Stripe checkout integration
   - Plan selection and billing setup
   - Payment confirmation and receipt
   - Immediate feature access restoration

4. **Account Limitation (If not subscribing)**
   - Profile becomes invisible to new matches
   - Existing conversations remain accessible (read-only)
   - Re-subscription option always available
   - Data retention for potential return

**Exit Criteria**: User either subscribes or enters limited access mode

---

### 5. Wali Account Setup & Management Flow

**Actor**: Guardian/Wali (Invited by primary user)

**Flow Steps**:
1. **Invitation Receipt**
   - Email invitation from family member
   - Explanation of role and Islamic importance
   - Account creation link with pre-filled family connection

2. **Wali Account Creation**
   - Simplified registration (name, email, relationship)
   - Islamic responsibility acknowledgment
   - Role-based permissions setup

3. **Dashboard Overview**
   - View connected family member's matching activity
   - Pending group chat invitations
   - Communication guidelines and best practices

4. **Group Chat Participation**
   - Receive notifications for new conversations
   - Participate in family introductions
   - Provide guidance and oversight
   - Ability to end conversations if concerns arise

**Exit Criteria**: Wali successfully participates in marriage discussions

---

### 6. Match Rejection & Algorithm Learning Flow

**Actor**: User experiencing multiple rejections

**Flow Steps**:
1. **Rejection Pattern Detection**
   - System tracks consecutive rejections (5+ in sequence)
   - Analysis of rejection reasons (if provided)
   - Compatibility score evaluation

2. **Feedback Collection Modal**
   - "We noticed you've rejected several matches"
   - Optional feedback on why matches weren't suitable
   - Suggestions for deal-breaker adjustment

3. **Deal-breaker Review Process**
   - Current filter review with usage statistics
   - "Most users with similar preferences accept X% of matches"
   - Guided filter adjustment recommendations

4. **Algorithm Optimization**
   - Updated preferences applied to matching algorithm
   - Improved match quality in next batch
   - User notification of changes and expected improvements

**Exit Criteria**: User adjusts preferences or continues with current settings

---

### 7. Profile Management & Updates Flow

**Actor**: Existing User (wanting to update information)

**Flow Steps**:
1. **Profile Access**
   - Navigate to profile settings
   - View current information summary
   - Identify sections available for editing

2. **Information Updates**
   - Basic demographics (limited changes)
   - Deal-breaker modifications
   - Wali contact information updates
   - Profile visibility settings

3. **Re-assessment Options**
   - Option to retake Enneagram test (once per 6 months)
   - Updated results integration
   - Impact preview on future matches

4. **Change Confirmation**
   - Review of all modifications
   - Impact warning for significant changes
   - Confirmation and profile update

**Exit Criteria**: Profile successfully updated with new information

---

## Error Handling & Edge Cases

### Common Error Flows

1. **Payment Failure During Subscription**
   - Retry payment with different method
   - Grace period extension (3 days)
   - Customer support contact option

2. **Technical Issues During Enneagram Test**
   - Progress saving and resume capability
   - Alternative test delivery method
   - Support ticket creation

3. **Inappropriate Communication in Group Chat**
   - Report functionality for all participants
   - Automatic moderation alerts
   - Admin review and potential account suspension

4. **Wali Unavailability**
   - Alternative guardian designation
   - Temporary delay in communication
   - Family coordination assistance

These user flows ensure a smooth, Islamic-compliant experience while maintaining technical reliability and user satisfaction throughout the marriage-seeking journey. 