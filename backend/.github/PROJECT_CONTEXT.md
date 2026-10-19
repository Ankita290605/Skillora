# Skillora - Course Platform Backend

## 🎯 Project Overview
**Skillora** is a comprehensive course-selling platform similar to Udemy, with an added certification feature upon course completion. Students can browse courses, enroll, track progress, and receive certificates when they complete courses.

## 🏗️ Architecture

### Tech Stack
- **Runtime**: Node.js (ES Modules)
- **Framework**: Express.js v5.2.1
- **Database**: MongoDB (Mongoose ODM v9.0.2)
- **Validation**: Zod v4.2.1
- **Authentication**: JWT (jsonwebtoken v9.0.3)
- **Security**: bcrypt v6.0.0
- **CORS**: Enabled for cross-origin requests

### Project Structure
```
backend/
├── model/              # Mongoose models
├── routes/             # API route definitions
├── controller/         # Business logic handlers
├── utils/              # Utility functions (ApiResponse, etc.)
├── db/                 # Database connection
├── index.js            # Entry point
└── package.json        # Dependencies & scripts
```

## 📊 Data Models

### 1. **User** (`user.model.js`)
Core user entity with role-based access control
- **Roles**: student, instructor, admin
- **Fields**: name, email, passwordHash, avatar, bio, isVerified
- **Features**: Email uniqueness, verification system
- **Timestamps**: Auto createdAt/updatedAt

### 2. **Course** (`course.model.js`)
Main course entity with comprehensive metadata
- **Content**: title, slug, description, thumbnail, previewVideo
- **Pricing**: price, isFree flag
- **Metadata**: level (beginner/intermediate/advanced), language, totalDuration
- **Relations**: instructor (User), category (Category)
- **Publishing**: published flag
- **Ratings**: rating average, ratingCount
- **Indexes**: slug (unique)

### 3. **Category** (`category.model.js`)
Course categorization system
- **Fields**: name, slug
- **Constraints**: Both name and slug are unique

### 4. **Section** (`section.model.js`)
Course content organization (modules/chapters)
- **Fields**: title, order
- **Relations**: course (Course)
- **Purpose**: Groups lectures into logical sections

### 5. **Lecture** (`lecture.model.js`)
Individual video lessons within sections
- **Fields**: title, videoUrl, duration, order
- **Relations**: section (Section)
- **Features**: isPreview flag for free preview lectures

### 6. **Enrollment** (`enrollment.model.js`)
Student course registration tracking
- **Relations**: user (User), course (Course)
- **Fields**: enrolledAt, completed (certification trigger)
- **Constraints**: Composite unique index (user + course) prevents duplicate enrollments

### 7. **Progress** (`progress.model.js`)
Tracks student learning progress per course
- **Relations**: user (User), course (Course)
- **Fields**: completedLectures[], progressPercentage
- **Purpose**: Enables resume-watching and progress tracking

### 8. **Review** (`review.model.js`)
Course ratings and feedback system
- **Relations**: user (User), course (Course)
- **Fields**: rating (1-5), comment
- **Constraints**: One review per user per course (composite unique index)
- **Timestamps**: Auto createdAt/updatedAt

### 9. **Payment** (`payment.model.js`)
Transaction and payment processing records
- **Relations**: user (User), course (Course)
- **Fields**: amount, currency (INR/USD), status (pending/success/failed/refunded)
- **Gateway**: orderId, transactionId, receipt
- **Features**: 
  - Prevents duplicate successful payments (composite unique index)
  - Failure tracking with failureReason
  - Metadata support for additional info
  - Strict mode prevents unknown fields

### 10. **Wishlist** (`wishlist.model.js`)
Student's saved courses for future purchase
- **Relations**: user (User, unique), courses[] (Course)
- **Purpose**: Save courses for later

## 🔄 Key Workflows

### Student Journey
1. **Registration** → User account creation (role: student)
2. **Browse** → View courses by category/search
3. **Wishlist** → Save interesting courses
4. **Purchase** → Payment processing (creates Payment record)
5. **Enrollment** → Access granted (creates Enrollment record)
6. **Learning** → Watch lectures, track Progress
7. **Completion** → Set enrollment.completed = true → **Issue Certificate**
8. **Review** → Rate and review the course

### Instructor Journey
1. **Registration** → User account (role: instructor)
2. **Create Course** → Add title, description, pricing
3. **Structure Content** → Create Sections
4. **Upload Lectures** → Add videos to Sections
5. **Publish** → Set course.published = true
6. **Monitor** → View enrollments, reviews, ratings

### Admin Journey
1. **Manage Categories** → CRUD operations
2. **Moderate Content** → Review courses before publishing
3. **Handle Payments** → Monitor transactions, refunds
4. **User Management** → Verify users, manage roles

## 🎓 Certification System
- **Trigger**: When `enrollment.completed` is set to `true`
- **Criteria**: Student completes all lectures (progressPercentage = 100%)
- **Implementation**: Certificate generation service (to be implemented)
- **Data**: User info + Course info + Completion date

## 🔐 Security Features
- Password hashing with bcrypt (strength: 6.0.0)
- JWT-based authentication
- Role-based authorization (student/instructor/admin)
- Email verification system (isVerified flag)
- Unique constraints prevent data duplication
- Indexed fields for faster queries
- Strict schema validation

## 📝 API Response Structure
Standardized using `utils/ApiResponse.js` with Zod validation:
```javascript
{
  success: boolean,
  message: string,
  data?: any,
  statusCode: number,
  timestamp: string (ISO 8601)
}
```

## 🚀 Scripts
```bash
npm run dev              # Start development server
npm run format           # Auto-format code with Prettier
npm run format:check     # Check code formatting
```

## 💡 Next Steps / TODO
- [ ] Implement certificate generation service
- [ ] Add file upload for videos/thumbnails (AWS S3/Cloudinary)
- [ ] Implement payment gateway integration (Razorpay/Stripe)
- [ ] Add email service for notifications
- [ ] Implement real-time progress updates
- [ ] Add course analytics for instructors
- [ ] Implement search functionality
- [ ] Add course recommendations
- [ ] Implement video streaming optimization
- [ ] Add quiz/assessment feature

## 📌 Important Notes
- Using ES6 modules (`type: "module"` in package.json)
- Environment variables loaded via `--env-file=.env`
- MongoDB indexes on frequently queried fields
- Composite indexes prevent duplicate entries
- Sparse indexes allow null values (e.g., transactionId)
- All models use timestamps for audit trails
