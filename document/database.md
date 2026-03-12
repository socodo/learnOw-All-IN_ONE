# Database Design - LearnOw Platform

## Quy ước đặt tên

| Loại | Quy ước | Ví dụ |
|------|---------|-------|
| Bảng | snake_case, số nhiều | `users`, `courses` |
| Cột | snake_case | `created_at`, `user_id` |
| Primary Key | `id` | UUID hoặc BIGSERIAL |
| Foreign Key | `<bảng_số_ít>_id` | `user_id`, `course_id` |
| Index | `idx_<bảng>_<cột>` | `idx_users_email` |

---

## Tổng quan các bảng theo Module

### Module 1: Authentication & User Management (6 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 1 | `users` | Thông tin người dùng |
| 2 | `roles` | Danh sách vai trò (learner, instructor, reviewer, admin) |
| 3 | `user_roles` | Liên kết user - role (many-to-many) |
| 4 | `oauth_accounts` | Tài khoản OAuth (Google, Facebook) |
| 5 | `refresh_tokens` | Refresh token cho JWT |
| 6 | `password_reset_tokens` | Token đặt lại mật khẩu |

### Module 2: Instructor Management (4 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 7 | `instructor_applications` | Đơn đăng ký làm giảng viên |
| 8 | `instructor_bank_accounts` | Thông tin tài khoản ngân hàng giảng viên |
| 9 | `instructor_earnings` | Doanh thu của giảng viên |
| 10 | `payout_requests` | Yêu cầu rút tiền |

### Module 3: Category (1 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 11 | `categories` | Danh mục khóa học (hỗ trợ cây phân cấp) |

### Module 4: Course Management (4 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 12 | `courses` | Thông tin khóa học |
| 13 | `sections` | Chương/phần trong khóa học |
| 14 | `lessons` | Bài học (video, bài viết, quiz, tài liệu) |
| 15 | `lesson_attachments` | Tệp đính kèm của bài học |

### Module 5: Quiz (3 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 16 | `quiz_questions` | Câu hỏi quiz |
| 17 | `quiz_options` | Đáp án của câu hỏi |
| 18 | `quiz_attempts` | Lịch sử làm quiz của học viên |

### Module 6: Course Review - Thẩm định (1 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 19 | `course_reviews` | Kết quả thẩm định khóa học bởi Reviewer |

### Module 7: Enrollment & Learning (4 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 20 | `enrollments` | Đăng ký học khóa học |
| 21 | `lesson_progress` | Tiến độ học từng bài |
| 22 | `user_notes` | Ghi chú cá nhân của học viên |
| 23 | `certificates` | Chứng chỉ hoàn thành |

### Module 8: Rating & Review - Đánh giá từ học viên (2 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 24 | `reviews` | Đánh giá khóa học từ học viên |
| 25 | `review_reports` | Báo cáo đánh giá vi phạm |

### Module 9: Order & Payment (3 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 26 | `orders` | Đơn hàng |
| 27 | `order_items` | Chi tiết đơn hàng |
| 28 | `payments` | Thanh toán |

### Module 10: Coupon & Discount (2 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 29 | `coupons` | Mã giảm giá |
| 30 | `coupon_usages` | Lịch sử sử dụng mã giảm giá |

### Module 11: Refund (1 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 31 | `refund_requests` | Yêu cầu hoàn tiền |

### Module 12: Notification (3 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 32 | `notifications` | Thông báo |
| 33 | `notification_templates` | Mẫu thông báo |
| 34 | `notification_preferences` | Cài đặt nhận thông báo |

### Module 13: System (3 bảng)

| # | Tên bảng | Mô tả |
|---|----------|-------|
| 35 | `system_configs` | Cấu hình hệ thống |
| 36 | `config_audit_logs` | Lịch sử thay đổi cấu hình |
| 37 | `audit_logs` | Nhật ký hoạt động hệ thống |

---

## Bảng tra cứu quan hệ giữa các Entity

### Quan hệ 1-N (One-to-Many)

| Bảng cha | Bảng con | Foreign Key | Mô tả |
|----------|----------|-------------|-------|
| `users` | `oauth_accounts` | user_id | User có nhiều OAuth accounts |
| `users` | `refresh_tokens` | user_id | User có nhiều refresh tokens |
| `users` | `password_reset_tokens` | user_id | User có nhiều password reset tokens |
| `users` | `instructor_applications` | user_id | User gửi nhiều đơn đăng ký giảng viên |
| `users` | `instructor_bank_accounts` | user_id | Instructor có nhiều tài khoản ngân hàng |
| `users` | `instructor_earnings` | instructor_id | Instructor có nhiều khoản thu nhập |
| `users` | `payout_requests` | instructor_id | Instructor có nhiều yêu cầu rút tiền |
| `users` | `courses` | instructor_id | Instructor tạo nhiều khóa học |
| `users` | `course_reviews` | reviewer_id | Reviewer thẩm định nhiều khóa học |
| `users` | `enrollments` | user_id | User đăng ký nhiều khóa học |
| `users` | `user_notes` | user_id | User tạo nhiều ghi chú |
| `users` | `reviews` | user_id | User viết nhiều đánh giá |
| `users` | `review_reports` | reported_by | User báo cáo nhiều đánh giá |
| `users` | `orders` | user_id | User có nhiều đơn hàng |
| `users` | `coupons` | created_by | User (Instructor/Admin) tạo nhiều coupon |
| `users` | `refund_requests` | user_id | User có nhiều yêu cầu hoàn tiền |
| `users` | `notifications` | user_id | User nhận nhiều thông báo |
| `users` | `notification_preferences` | user_id | User có nhiều cài đặt thông báo |
| `users` | `audit_logs` | user_id | User tạo nhiều audit logs |
| `users` | `config_audit_logs` | changed_by | User thay đổi nhiều cấu hình |
| `roles` | `user_roles` | role_id | Role được gán cho nhiều users |
| `categories` | `categories` | parent_id | Category có nhiều sub-categories |
| `categories` | `courses` | category_id | Category chứa nhiều khóa học |
| `courses` | `sections` | course_id | Course có nhiều sections |
| `courses` | `course_reviews` | course_id | Course có nhiều lần thẩm định |
| `courses` | `enrollments` | course_id | Course có nhiều enrollments |
| `courses` | `reviews` | course_id | Course có nhiều đánh giá |
| `courses` | `coupons` | course_id | Course có nhiều coupons |
| `sections` | `lessons` | section_id | Section có nhiều lessons |
| `lessons` | `lesson_attachments` | lesson_id | Lesson có nhiều attachments |
| `lessons` | `quiz_questions` | lesson_id | Lesson (quiz) có nhiều câu hỏi |
| `lessons` | `lesson_progress` | lesson_id | Lesson có nhiều progress records |
| `lessons` | `user_notes` | lesson_id | Lesson có nhiều ghi chú |
| `quiz_questions` | `quiz_options` | question_id | Question có nhiều options |
| `quiz_questions` | `quiz_attempts` | question_id | Question có nhiều attempts |
| `enrollments` | `lesson_progress` | enrollment_id | Enrollment có nhiều lesson progress |
| `enrollments` | `quiz_attempts` | enrollment_id | Enrollment có nhiều quiz attempts |
| `reviews` | `review_reports` | review_id | Review có nhiều reports |
| `orders` | `order_items` | order_id | Order có nhiều items |
| `orders` | `payments` | order_id | Order có nhiều payments |
| `orders` | `coupon_usages` | order_id | Order sử dụng nhiều coupons |
| `coupons` | `coupon_usages` | coupon_id | Coupon có nhiều lần sử dụng |
| `instructor_bank_accounts` | `payout_requests` | bank_account_id | Bank account có nhiều payout requests |
| `payout_requests` | `instructor_earnings` | payout_request_id | Payout request chứa nhiều earnings |
| `notification_templates` | `notifications` | template_id | Template được dùng cho nhiều notifications |

### Quan hệ 1-1 (One-to-One)

| Bảng A | Bảng B | Foreign Key | Mô tả |
|--------|--------|-------------|-------|
| `enrollments` | `certificates` | enrollment_id | Mỗi enrollment có tối đa 1 certificate |
| `orders` | `refund_requests` | order_id | Mỗi order có tối đa 1 refund request |

### Quan hệ N-N (Many-to-Many) thông qua bảng trung gian

| Bảng A | Bảng B | Bảng trung gian | Mô tả |
|--------|--------|-----------------|-------|
| `users` | `roles` | `user_roles` | User có nhiều roles, Role gán cho nhiều users |
| `users` | `coupons` | `coupon_usages` | User sử dụng nhiều coupons, Coupon được nhiều users dùng |
| `order_items` | `coupons` | (FK trong order_items) | Order item áp dụng coupon |
| `order_items` | `enrollments` | (FK trong enrollments) | Order item tạo enrollment |

### Quan hệ tự tham chiếu (Self-referencing)

| Bảng | Foreign Key | Mô tả |
|------|-------------|-------|
| `categories` | parent_id → categories.id | Danh mục cha - con |
| `users` | (instructor_applications.reviewed_by → users.id) | Admin duyệt đơn |
| `users` | (user_roles.assigned_by → users.id) | Người gán vai trò |
| `users` | (refund_requests.reviewed_by → users.id) | Admin xử lý hoàn tiền |
| `users` | (payout_requests.processed_by → users.id) | Admin xử lý payout |
| `users` | (review_reports.resolved_by → users.id) | Admin xử lý report |

---

## ERD Diagram (Eraser.io)

```eraser
// LearnOw Platform - Entity Relationship Diagram
// Copy this code to eraser.io

colorMode pastel
styleMode shadow
typeface clean

// ==========================================
// MODULE 1: AUTHENTICATION & USER MANAGEMENT
// ==========================================

users [icon: user, color: blue] {
  id uuid pk
  email varchar uk
  password_hash varchar
  full_name varchar
  avatar_url varchar
  bio text
  phone varchar
  social_links jsonb
  status enum
  email_verified_at timestamp
  failed_login_attempts int
  locked_until timestamp
  last_login_at timestamp
  created_at timestamp
  updated_at timestamp
  deleted_at timestamp
}

roles [icon: shield, color: blue] {
  id serial pk
  name varchar uk
  description text
  created_at timestamp
}

user_roles [icon: link, color: blue] {
  id bigserial pk
  user_id uuid fk
  role_id int fk
  assigned_at timestamp
  assigned_by uuid fk
}

oauth_accounts [icon: key, color: blue] {
  id bigserial pk
  user_id uuid fk
  provider varchar
  provider_user_id varchar
  access_token text
  refresh_token text
  expires_at timestamp
  created_at timestamp
  updated_at timestamp
}

refresh_tokens [icon: refresh-cw, color: blue] {
  id uuid pk
  user_id uuid fk
  token_hash varchar
  expires_at timestamp
  revoked_at timestamp
  created_at timestamp
  user_agent text
  ip_address inet
}

password_reset_tokens [icon: lock, color: blue] {
  id uuid pk
  user_id uuid fk
  token_hash varchar
  expires_at timestamp
  used_at timestamp
  created_at timestamp
}

// ==========================================
// MODULE 2: INSTRUCTOR MANAGEMENT
// ==========================================

instructor_applications [icon: file-text, color: green] {
  id uuid pk
  user_id uuid fk
  expertise_area varchar
  experience text
  qualifications text
  demo_course_url varchar
  status enum
  reviewed_by uuid fk
  reviewed_at timestamp
  rejection_reason text
  created_at timestamp
  updated_at timestamp
}

instructor_bank_accounts [icon: credit-card, color: green] {
  id uuid pk
  user_id uuid fk
  bank_name varchar
  account_number varchar
  account_holder_name varchar
  branch varchar
  is_primary boolean
  created_at timestamp
  updated_at timestamp
}

instructor_earnings [icon: dollar-sign, color: green] {
  id uuid pk
  instructor_id uuid fk
  order_item_id uuid fk
  course_id uuid fk
  gross_amount decimal
  platform_fee_percent decimal
  platform_fee_amount decimal
  net_amount decimal
  is_confirmed boolean
  confirmed_at timestamp
  is_paid boolean
  paid_at timestamp
  payout_request_id uuid fk
  created_at timestamp
}

payout_requests [icon: send, color: green] {
  id uuid pk
  instructor_id uuid fk
  bank_account_id uuid fk
  amount decimal
  status enum
  processed_by uuid fk
  processed_at timestamp
  transaction_reference varchar
  failure_reason text
  created_at timestamp
  updated_at timestamp
}

// ==========================================
// MODULE 3: CATEGORY
// ==========================================

categories [icon: folder, color: purple] {
  id serial pk
  parent_id int fk
  name varchar
  slug varchar uk
  description text
  icon_url varchar
  sort_order int
  is_active boolean
  created_at timestamp
  updated_at timestamp
}

// ==========================================
// MODULE 4: COURSE MANAGEMENT
// ==========================================

courses [icon: book-open, color: orange] {
  id uuid pk
  instructor_id uuid fk
  category_id int fk
  title varchar
  slug varchar uk
  short_description varchar
  full_description text
  thumbnail_url varchar
  preview_video_url varchar
  language varchar
  level enum
  price decimal
  original_price decimal
  requirements text_array
  learning_objectives text_array
  status enum
  published_at timestamp
  total_duration_minutes int
  total_lessons int
  total_students int
  average_rating decimal
  total_reviews int
  version int
  created_at timestamp
  updated_at timestamp
  deleted_at timestamp
}

sections [icon: list, color: orange] {
  id uuid pk
  course_id uuid fk
  title varchar
  description text
  sort_order int
  created_at timestamp
  updated_at timestamp
}

lessons [icon: play-circle, color: orange] {
  id uuid pk
  section_id uuid fk
  title varchar
  description text
  lesson_type enum
  content_url varchar
  content_text text
  duration_minutes int
  is_preview boolean
  sort_order int
  created_at timestamp
  updated_at timestamp
}

lesson_attachments [icon: paperclip, color: orange] {
  id uuid pk
  lesson_id uuid fk
  file_name varchar
  file_url varchar
  file_size_bytes bigint
  file_type varchar
  created_at timestamp
}

// ==========================================
// MODULE 5: QUIZ
// ==========================================

quiz_questions [icon: help-circle, color: yellow] {
  id uuid pk
  lesson_id uuid fk
  question_text text
  explanation text
  sort_order int
  points int
  created_at timestamp
  updated_at timestamp
}

quiz_options [icon: check-square, color: yellow] {
  id uuid pk
  question_id uuid fk
  option_text text
  is_correct boolean
  sort_order int
}

quiz_attempts [icon: edit-3, color: yellow] {
  id uuid pk
  enrollment_id uuid fk
  lesson_id uuid fk
  question_id uuid fk
  selected_option_id uuid fk
  is_correct boolean
  attempted_at timestamp
}

// ==========================================
// MODULE 6: COURSE REVIEW (THAM DINH)
// ==========================================

course_reviews [icon: clipboard-check, color: pink] {
  id uuid pk
  course_id uuid fk
  reviewer_id uuid fk
  decision enum
  content_quality_score int
  structure_score int
  video_quality_score int
  materials_score int
  metadata_score int
  weighted_average decimal
  overall_comment text
  rejection_reason text
  improvement_suggestions text
  created_at timestamp
}

// ==========================================
// MODULE 7: ENROLLMENT & LEARNING
// ==========================================

enrollments [icon: user-check, color: cyan] {
  id uuid pk
  user_id uuid fk
  course_id uuid fk
  order_item_id uuid fk
  enrolled_at timestamp
  progress_percent int
  completed_at timestamp
  last_accessed_at timestamp
}

lesson_progress [icon: trending-up, color: cyan] {
  id uuid pk
  enrollment_id uuid fk
  lesson_id uuid fk
  is_completed boolean
  completed_at timestamp
  video_position_seconds int
  updated_at timestamp
}

user_notes [icon: edit, color: cyan] {
  id uuid pk
  user_id uuid fk
  lesson_id uuid fk
  content text
  timestamp_seconds int
  created_at timestamp
  updated_at timestamp
}

certificates [icon: award, color: cyan] {
  id uuid pk
  enrollment_id uuid fk
  certificate_number varchar uk
  issued_at timestamp
  pdf_url varchar
  verification_url varchar
}

// ==========================================
// MODULE 8: RATING & REVIEW
// ==========================================

reviews [icon: star, color: red] {
  id uuid pk
  user_id uuid fk
  course_id uuid fk
  rating int
  comment text
  is_visible boolean
  reported_count int
  created_at timestamp
  updated_at timestamp
}

review_reports [icon: flag, color: red] {
  id uuid pk
  review_id uuid fk
  reported_by uuid fk
  reason varchar
  is_resolved boolean
  resolved_by uuid fk
  resolution_note text
  created_at timestamp
  resolved_at timestamp
}

// ==========================================
// MODULE 9: ORDER & PAYMENT
// ==========================================

orders [icon: shopping-cart, color: teal] {
  id uuid pk
  user_id uuid fk
  order_number varchar uk
  subtotal decimal
  discount_amount decimal
  total_amount decimal
  status enum
  expires_at timestamp
  completed_at timestamp
  created_at timestamp
  updated_at timestamp
}

order_items [icon: package, color: teal] {
  id uuid pk
  order_id uuid fk
  course_id uuid fk
  price decimal
  discount_amount decimal
  final_price decimal
  coupon_id uuid fk
  created_at timestamp
}

payments [icon: credit-card, color: teal] {
  id uuid pk
  order_id uuid fk
  payment_method varchar
  payment_gateway varchar
  gateway_transaction_id varchar
  amount decimal
  status varchar
  gateway_response jsonb
  paid_at timestamp
  created_at timestamp
  updated_at timestamp
}

// ==========================================
// MODULE 10: COUPON & DISCOUNT
// ==========================================

coupons [icon: tag, color: lime] {
  id uuid pk
  code varchar uk
  discount_type enum
  discount_value decimal
  max_discount_amount decimal
  min_order_amount decimal
  course_id uuid fk
  created_by uuid fk
  max_uses int
  current_uses int
  starts_at timestamp
  expires_at timestamp
  is_active boolean
  created_at timestamp
  updated_at timestamp
}

coupon_usages [icon: check-circle, color: lime] {
  id uuid pk
  coupon_id uuid fk
  user_id uuid fk
  order_id uuid fk
  used_at timestamp
}

// ==========================================
// MODULE 11: REFUND
// ==========================================

refund_requests [icon: rotate-ccw, color: brown] {
  id uuid pk
  order_id uuid fk
  user_id uuid fk
  amount decimal
  reason text
  progress_at_request int
  status enum
  reviewed_by uuid fk
  reviewed_at timestamp
  rejection_reason text
  refunded_at timestamp
  created_at timestamp
  updated_at timestamp
}

// ==========================================
// MODULE 12: NOTIFICATION
// ==========================================

notifications [icon: bell, color: indigo] {
  id uuid pk
  user_id uuid fk
  template_id int fk
  title varchar
  body text
  data jsonb
  channel enum
  is_read boolean
  read_at timestamp
  email_sent_at timestamp
  created_at timestamp
}

notification_templates [icon: file-text, color: indigo] {
  id serial pk
  code varchar uk
  title_template text
  body_template text
  channel enum
  created_at timestamp
}

notification_preferences [icon: settings, color: indigo] {
  id uuid pk
  user_id uuid fk
  template_code varchar
  email_enabled boolean
  in_app_enabled boolean
  updated_at timestamp
}

// ==========================================
// MODULE 13: SYSTEM
// ==========================================

system_configs [icon: sliders, color: gray] {
  id serial pk
  key varchar uk
  value text
  value_type varchar
  description text
  updated_by uuid fk
  updated_at timestamp
}

config_audit_logs [icon: file-text, color: gray] {
  id uuid pk
  config_key varchar
  old_value text
  new_value text
  changed_by uuid fk
  changed_at timestamp
}

audit_logs [icon: activity, color: gray] {
  id uuid pk
  user_id uuid fk
  action varchar
  entity_type varchar
  entity_id uuid
  old_values jsonb
  new_values jsonb
  ip_address inet
  user_agent text
  created_at timestamp
}

// ==========================================
// RELATIONSHIPS
// ==========================================

// Module 1: Auth
users.id < user_roles.user_id
roles.id < user_roles.role_id
users.id < user_roles.assigned_by
users.id < oauth_accounts.user_id
users.id < refresh_tokens.user_id
users.id < password_reset_tokens.user_id

// Module 2: Instructor
users.id < instructor_applications.user_id
users.id < instructor_applications.reviewed_by
users.id < instructor_bank_accounts.user_id
users.id < instructor_earnings.instructor_id
users.id < payout_requests.instructor_id
users.id < payout_requests.processed_by
instructor_bank_accounts.id < payout_requests.bank_account_id
payout_requests.id < instructor_earnings.payout_request_id

// Module 3: Category
categories.id < categories.parent_id

// Module 4: Course
users.id < courses.instructor_id
categories.id < courses.category_id
courses.id < sections.course_id
sections.id < lessons.section_id
lessons.id < lesson_attachments.lesson_id

// Module 5: Quiz
lessons.id < quiz_questions.lesson_id
quiz_questions.id < quiz_options.question_id
quiz_questions.id < quiz_attempts.question_id
quiz_options.id < quiz_attempts.selected_option_id

// Module 6: Course Review
courses.id < course_reviews.course_id
users.id < course_reviews.reviewer_id

// Module 7: Enrollment
users.id < enrollments.user_id
courses.id < enrollments.course_id
enrollments.id < lesson_progress.enrollment_id
lessons.id < lesson_progress.lesson_id
enrollments.id < quiz_attempts.enrollment_id
lessons.id < quiz_attempts.lesson_id
users.id < user_notes.user_id
lessons.id < user_notes.lesson_id
enrollments.id - certificates.enrollment_id

// Module 8: Rating
users.id < reviews.user_id
courses.id < reviews.course_id
reviews.id < review_reports.review_id
users.id < review_reports.reported_by
users.id < review_reports.resolved_by

// Module 9: Order
users.id < orders.user_id
orders.id < order_items.order_id
courses.id < order_items.course_id
orders.id < payments.order_id
order_items.id < enrollments.order_item_id
order_items.id < instructor_earnings.order_item_id
courses.id < instructor_earnings.course_id

// Module 10: Coupon
users.id < coupons.created_by
courses.id < coupons.course_id
coupons.id < coupon_usages.coupon_id
users.id < coupon_usages.user_id
orders.id < coupon_usages.order_id
coupons.id < order_items.coupon_id

// Module 11: Refund
orders.id - refund_requests.order_id
users.id < refund_requests.user_id
users.id < refund_requests.reviewed_by

// Module 12: Notification
users.id < notifications.user_id
notification_templates.id < notifications.template_id
users.id < notification_preferences.user_id

// Module 13: System
users.id < system_configs.updated_by
users.id < config_audit_logs.changed_by
users.id < audit_logs.user_id
```

---

## Chi tiết các bảng theo Module

---

### MODULE 1: Authentication & User Management

#### 1.1 users

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| email | VARCHAR(255) | NOT NULL, UNIQUE | Email đăng nhập |
| password_hash | VARCHAR(255) | NULL | Mật khẩu đã hash (NULL nếu OAuth) |
| full_name | VARCHAR(255) | NOT NULL | Họ tên |
| avatar_url | VARCHAR(500) | NULL | URL ảnh đại diện |
| bio | TEXT | NULL | Giới thiệu bản thân |
| phone | VARCHAR(20) | NULL | Số điện thoại |
| social_links | JSONB | DEFAULT '{}' | Liên kết mạng xã hội |
| status | ENUM | NOT NULL, DEFAULT 'pending_verification' | Trạng thái tài khoản |
| email_verified_at | TIMESTAMP | NULL | Thời điểm xác thực email |
| failed_login_attempts | INT | DEFAULT 0 | Số lần đăng nhập sai |
| locked_until | TIMESTAMP | NULL | Khóa tài khoản đến khi nào |
| last_login_at | TIMESTAMP | NULL | Lần đăng nhập cuối |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |
| deleted_at | TIMESTAMP | NULL | Soft delete |

#### 1.2 roles

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | SERIAL | PK | Khóa chính |
| name | VARCHAR(50) | NOT NULL, UNIQUE | Tên vai trò |
| description | TEXT | NULL | Mô tả |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

#### 1.3 user_roles

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | BIGSERIAL | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User |
| role_id | INT | FK → roles, NOT NULL | Role |
| assigned_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày gán vai trò |
| assigned_by | UUID | FK → users, NULL | Người gán |

**Unique constraint:** (user_id, role_id)

#### 1.4 oauth_accounts

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | BIGSERIAL | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User |
| provider | VARCHAR(50) | NOT NULL | google, facebook |
| provider_user_id | VARCHAR(255) | NOT NULL | ID từ provider |
| access_token | TEXT | NULL | Access token |
| refresh_token | TEXT | NULL | Refresh token |
| expires_at | TIMESTAMP | NULL | Hết hạn |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

**Unique constraint:** (provider, provider_user_id)

#### 1.5 refresh_tokens

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User |
| token_hash | VARCHAR(255) | NOT NULL | Token đã hash |
| expires_at | TIMESTAMP | NOT NULL | Hết hạn |
| revoked_at | TIMESTAMP | NULL | Thời điểm thu hồi |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| user_agent | TEXT | NULL | Thiết bị |
| ip_address | INET | NULL | Địa chỉ IP |

#### 1.6 password_reset_tokens

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User |
| token_hash | VARCHAR(255) | NOT NULL | Token đã hash |
| expires_at | TIMESTAMP | NOT NULL | Hết hạn (30 phút) |
| used_at | TIMESTAMP | NULL | Đã sử dụng lúc |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

---

### MODULE 2: Instructor Management

#### 2.1 instructor_applications

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User gửi đơn |
| expertise_area | VARCHAR(255) | NOT NULL | Lĩnh vực chuyên môn |
| experience | TEXT | NOT NULL | Kinh nghiệm |
| qualifications | TEXT | NULL | Bằng cấp/chứng chỉ |
| demo_course_url | VARCHAR(500) | NULL | URL khóa học demo |
| status | ENUM | NOT NULL, DEFAULT 'pending' | pending, approved, rejected |
| reviewed_by | UUID | FK → users, NULL | Admin duyệt |
| reviewed_at | TIMESTAMP | NULL | Ngày duyệt |
| rejection_reason | TEXT | NULL | Lý do từ chối |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 2.2 instructor_bank_accounts

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | Instructor |
| bank_name | VARCHAR(100) | NOT NULL | Tên ngân hàng |
| account_number | VARCHAR(50) | NOT NULL | Số tài khoản |
| account_holder_name | VARCHAR(255) | NOT NULL | Tên chủ tài khoản |
| branch | VARCHAR(255) | NULL | Chi nhánh |
| is_primary | BOOLEAN | DEFAULT false | Tài khoản chính |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 2.3 instructor_earnings

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| instructor_id | UUID | FK → users, NOT NULL | Instructor |
| order_item_id | UUID | FK → order_items, NOT NULL | Order item |
| course_id | UUID | FK → courses, NOT NULL | Khóa học |
| gross_amount | DECIMAL(15,2) | NOT NULL | Giá bán |
| platform_fee_percent | DECIMAL(5,2) | NOT NULL | % phí nền tảng |
| platform_fee_amount | DECIMAL(15,2) | NOT NULL | Số tiền phí |
| net_amount | DECIMAL(15,2) | NOT NULL | Số tiền instructor nhận |
| is_confirmed | BOOLEAN | DEFAULT false | Đã xác nhận (sau 7 ngày) |
| confirmed_at | TIMESTAMP | NULL | Ngày xác nhận |
| is_paid | BOOLEAN | DEFAULT false | Đã thanh toán |
| paid_at | TIMESTAMP | NULL | Ngày thanh toán |
| payout_request_id | UUID | FK → payout_requests, NULL | Thuộc payout nào |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

#### 2.4 payout_requests

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| instructor_id | UUID | FK → users, NOT NULL | Instructor |
| bank_account_id | UUID | FK → instructor_bank_accounts, NOT NULL | Tài khoản ngân hàng |
| amount | DECIMAL(15,2) | NOT NULL, CHECK (≥ 500000) | Số tiền rút |
| status | ENUM | NOT NULL, DEFAULT 'pending' | pending, processing, completed, failed |
| processed_by | UUID | FK → users, NULL | Admin xử lý |
| processed_at | TIMESTAMP | NULL | Ngày xử lý |
| transaction_reference | VARCHAR(255) | NULL | Mã giao dịch |
| failure_reason | TEXT | NULL | Lý do thất bại |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

---

### MODULE 3: Category

#### 3.1 categories

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | SERIAL | PK | Khóa chính |
| parent_id | INT | FK → categories, NULL | Danh mục cha (NULL nếu root) |
| name | VARCHAR(100) | NOT NULL | Tên danh mục |
| slug | VARCHAR(100) | NOT NULL, UNIQUE | Slug URL |
| description | TEXT | NULL | Mô tả |
| icon_url | VARCHAR(500) | NULL | URL icon |
| sort_order | INT | DEFAULT 0 | Thứ tự sắp xếp |
| is_active | BOOLEAN | DEFAULT true | Đang hoạt động |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

**Unique constraint:** (parent_id, name)

---

### MODULE 4: Course Management

#### 4.1 courses

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| instructor_id | UUID | FK → users, NOT NULL | Giảng viên |
| category_id | INT | FK → categories, NOT NULL | Danh mục |
| title | VARCHAR(100) | NOT NULL, CHECK (10-100 chars) | Tên khóa học |
| slug | VARCHAR(150) | NOT NULL, UNIQUE | Slug URL |
| short_description | VARCHAR(200) | NOT NULL | Mô tả ngắn |
| full_description | TEXT | NOT NULL | Mô tả chi tiết |
| thumbnail_url | VARCHAR(500) | NULL | Ảnh thumbnail |
| preview_video_url | VARCHAR(500) | NULL | Video giới thiệu |
| language | VARCHAR(10) | NOT NULL, DEFAULT 'vi' | Ngôn ngữ |
| level | ENUM | NOT NULL, DEFAULT 'beginner' | beginner, intermediate, advanced |
| price | DECIMAL(15,2) | NOT NULL, DEFAULT 0, CHECK (0-10000000) | Giá |
| original_price | DECIMAL(15,2) | NULL | Giá gốc |
| requirements | TEXT[] | NULL | Yêu cầu tiên quyết |
| learning_objectives | TEXT[] | NULL | Mục tiêu học tập |
| status | ENUM | NOT NULL, DEFAULT 'draft' | Trạng thái khóa học |
| published_at | TIMESTAMP | NULL | Ngày xuất bản |
| total_duration_minutes | INT | DEFAULT 0 | Tổng thời lượng (cached) |
| total_lessons | INT | DEFAULT 0 | Tổng số bài học (cached) |
| total_students | INT | DEFAULT 0 | Số học viên (cached) |
| average_rating | DECIMAL(2,1) | DEFAULT 0 | Điểm đánh giá TB (cached) |
| total_reviews | INT | DEFAULT 0 | Số lượt đánh giá (cached) |
| version | INT | DEFAULT 1 | Phiên bản |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |
| deleted_at | TIMESTAMP | NULL | Soft delete |

#### 4.2 sections

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| course_id | UUID | FK → courses, NOT NULL | Khóa học |
| title | VARCHAR(200) | NOT NULL | Tên chương |
| description | TEXT | NULL | Mô tả |
| sort_order | INT | NOT NULL, DEFAULT 0 | Thứ tự |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 4.3 lessons

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| section_id | UUID | FK → sections, NOT NULL | Section |
| title | VARCHAR(200) | NOT NULL | Tên bài học |
| description | TEXT | NULL | Mô tả |
| lesson_type | ENUM | NOT NULL | video, article, quiz, document |
| content_url | VARCHAR(500) | NULL | URL video/tài liệu |
| content_text | TEXT | NULL | Nội dung bài viết |
| duration_minutes | INT | DEFAULT 0 | Thời lượng (phút) |
| is_preview | BOOLEAN | DEFAULT false | Cho xem miễn phí |
| sort_order | INT | NOT NULL, DEFAULT 0 | Thứ tự |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 4.4 lesson_attachments

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| lesson_id | UUID | FK → lessons, NOT NULL | Bài học |
| file_name | VARCHAR(255) | NOT NULL | Tên file |
| file_url | VARCHAR(500) | NOT NULL | URL file |
| file_size_bytes | BIGINT | NULL | Kích thước |
| file_type | VARCHAR(100) | NULL | Loại file |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

---

### MODULE 5: Quiz

#### 5.1 quiz_questions

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| lesson_id | UUID | FK → lessons, NOT NULL | Bài học (quiz) |
| question_text | TEXT | NOT NULL | Nội dung câu hỏi |
| explanation | TEXT | NULL | Giải thích đáp án |
| sort_order | INT | DEFAULT 0 | Thứ tự |
| points | INT | DEFAULT 1 | Điểm |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 5.2 quiz_options

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| question_id | UUID | FK → quiz_questions, NOT NULL | Câu hỏi |
| option_text | TEXT | NOT NULL | Nội dung đáp án |
| is_correct | BOOLEAN | NOT NULL, DEFAULT false | Đáp án đúng |
| sort_order | INT | DEFAULT 0 | Thứ tự |

#### 5.3 quiz_attempts

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| enrollment_id | UUID | FK → enrollments, NOT NULL | Enrollment |
| lesson_id | UUID | FK → lessons, NOT NULL | Bài quiz |
| question_id | UUID | FK → quiz_questions, NOT NULL | Câu hỏi |
| selected_option_id | UUID | FK → quiz_options, NULL | Đáp án đã chọn |
| is_correct | BOOLEAN | NULL | Trả lời đúng |
| attempted_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Thời điểm làm |

---

### MODULE 6: Course Review (Thẩm định)

#### 6.1 course_reviews

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| course_id | UUID | FK → courses, NOT NULL | Khóa học |
| reviewer_id | UUID | FK → users, NOT NULL | Reviewer |
| decision | ENUM | NOT NULL | approved, rejected, needs_revision |
| content_quality_score | INT | NOT NULL, CHECK (1-5) | Điểm chất lượng nội dung |
| structure_score | INT | NOT NULL, CHECK (1-5) | Điểm cấu trúc |
| video_quality_score | INT | NOT NULL, CHECK (1-5) | Điểm chất lượng video |
| materials_score | INT | NOT NULL, CHECK (1-5) | Điểm tài liệu |
| metadata_score | INT | NOT NULL, CHECK (1-5) | Điểm metadata |
| weighted_average | DECIMAL(3,2) | NULL | Điểm TB có trọng số |
| overall_comment | TEXT | NOT NULL | Nhận xét tổng thể |
| rejection_reason | TEXT | NULL | Lý do từ chối |
| improvement_suggestions | TEXT | NULL | Gợi ý cải thiện |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

---

### MODULE 7: Enrollment & Learning

#### 7.1 enrollments

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | Học viên |
| course_id | UUID | FK → courses, NOT NULL | Khóa học |
| order_item_id | UUID | FK → order_items, NULL | Order item (NULL nếu miễn phí) |
| enrolled_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày đăng ký |
| progress_percent | INT | DEFAULT 0 | Tiến độ % |
| completed_at | TIMESTAMP | NULL | Ngày hoàn thành |
| last_accessed_at | TIMESTAMP | NULL | Truy cập gần nhất |

**Unique constraint:** (user_id, course_id)

#### 7.2 lesson_progress

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| enrollment_id | UUID | FK → enrollments, NOT NULL | Enrollment |
| lesson_id | UUID | FK → lessons, NOT NULL | Bài học |
| is_completed | BOOLEAN | DEFAULT false | Đã hoàn thành |
| completed_at | TIMESTAMP | NULL | Ngày hoàn thành |
| video_position_seconds | INT | DEFAULT 0 | Vị trí video đang xem |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

**Unique constraint:** (enrollment_id, lesson_id)

#### 7.3 user_notes

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User |
| lesson_id | UUID | FK → lessons, NOT NULL | Bài học |
| content | TEXT | NOT NULL | Nội dung ghi chú |
| timestamp_seconds | INT | NULL | Thời điểm trong video |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 7.4 certificates

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| enrollment_id | UUID | FK → enrollments, NOT NULL, UNIQUE | Enrollment |
| certificate_number | VARCHAR(50) | NOT NULL, UNIQUE | Mã chứng chỉ |
| issued_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cấp |
| pdf_url | VARCHAR(500) | NULL | URL file PDF |
| verification_url | VARCHAR(500) | NULL | URL xác thực |

---

### MODULE 8: Rating & Review (Đánh giá từ học viên)

#### 8.1 reviews

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | Học viên |
| course_id | UUID | FK → courses, NOT NULL | Khóa học |
| rating | INT | NOT NULL, CHECK (1-5) | Số sao |
| comment | TEXT | NOT NULL, CHECK (10-1000 chars) | Nhận xét |
| is_visible | BOOLEAN | DEFAULT true | Hiển thị |
| reported_count | INT | DEFAULT 0 | Số lần bị báo cáo |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

**Unique constraint:** (user_id, course_id)

#### 8.2 review_reports

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| review_id | UUID | FK → reviews, NOT NULL | Review bị báo cáo |
| reported_by | UUID | FK → users, NOT NULL | Người báo cáo |
| reason | VARCHAR(255) | NOT NULL | Lý do báo cáo |
| is_resolved | BOOLEAN | DEFAULT false | Đã xử lý |
| resolved_by | UUID | FK → users, NULL | Admin xử lý |
| resolution_note | TEXT | NULL | Ghi chú xử lý |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| resolved_at | TIMESTAMP | NULL | Ngày xử lý |

---

### MODULE 9: Order & Payment

#### 9.1 orders

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | Người mua |
| order_number | VARCHAR(50) | NOT NULL, UNIQUE | Mã đơn hàng |
| subtotal | DECIMAL(15,2) | NOT NULL | Tổng tiền trước giảm |
| discount_amount | DECIMAL(15,2) | DEFAULT 0 | Số tiền giảm |
| total_amount | DECIMAL(15,2) | NOT NULL | Tổng thanh toán |
| status | ENUM | NOT NULL, DEFAULT 'pending' | pending, completed, failed, cancelled, refunded |
| expires_at | TIMESTAMP | NULL | Hết hạn (30 phút) |
| completed_at | TIMESTAMP | NULL | Ngày hoàn thành |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 9.2 order_items

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| order_id | UUID | FK → orders, NOT NULL | Đơn hàng |
| course_id | UUID | FK → courses, NOT NULL | Khóa học |
| price | DECIMAL(15,2) | NOT NULL | Giá gốc |
| discount_amount | DECIMAL(15,2) | DEFAULT 0 | Số tiền giảm |
| final_price | DECIMAL(15,2) | NOT NULL | Giá cuối cùng |
| coupon_id | UUID | FK → coupons, NULL | Coupon đã áp dụng |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

#### 9.3 payments

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| order_id | UUID | FK → orders, NOT NULL | Đơn hàng |
| payment_method | VARCHAR(50) | NOT NULL | momo, zalopay, vnpay, bank_transfer, visa |
| payment_gateway | VARCHAR(50) | NOT NULL | Cổng thanh toán |
| gateway_transaction_id | VARCHAR(255) | NULL | Mã giao dịch từ gateway |
| amount | DECIMAL(15,2) | NOT NULL | Số tiền |
| status | VARCHAR(50) | NOT NULL, DEFAULT 'pending' | Trạng thái |
| gateway_response | JSONB | NULL | Response từ gateway |
| paid_at | TIMESTAMP | NULL | Ngày thanh toán |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

---

### MODULE 10: Coupon & Discount

#### 10.1 coupons

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| code | VARCHAR(50) | NOT NULL, UNIQUE | Mã coupon |
| discount_type | ENUM | NOT NULL | percentage, fixed_amount |
| discount_value | DECIMAL(15,2) | NOT NULL | Giá trị giảm |
| max_discount_amount | DECIMAL(15,2) | NULL | Giảm tối đa (cho %) |
| min_order_amount | DECIMAL(15,2) | DEFAULT 0 | Đơn hàng tối thiểu |
| course_id | UUID | FK → courses, NULL | Áp dụng cho khóa học (NULL = toàn hệ thống) |
| created_by | UUID | FK → users, NOT NULL | Người tạo |
| max_uses | INT | NULL | Số lượt dùng tối đa |
| current_uses | INT | DEFAULT 0 | Đã sử dụng |
| starts_at | TIMESTAMP | NOT NULL | Bắt đầu |
| expires_at | TIMESTAMP | NOT NULL | Hết hạn |
| is_active | BOOLEAN | DEFAULT true | Đang hoạt động |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

**Check constraint:** expires_at > starts_at

#### 10.2 coupon_usages

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| coupon_id | UUID | FK → coupons, NOT NULL | Coupon |
| user_id | UUID | FK → users, NOT NULL | User sử dụng |
| order_id | UUID | FK → orders, NOT NULL | Đơn hàng |
| used_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày sử dụng |

**Unique constraint:** (coupon_id, user_id, order_id)

---

### MODULE 11: Refund

#### 11.1 refund_requests

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| order_id | UUID | FK → orders, NOT NULL | Đơn hàng |
| user_id | UUID | FK → users, NOT NULL | Người yêu cầu |
| amount | DECIMAL(15,2) | NOT NULL | Số tiền hoàn |
| reason | TEXT | NOT NULL | Lý do |
| progress_at_request | INT | NOT NULL | Tiến độ học khi yêu cầu |
| status | ENUM | NOT NULL, DEFAULT 'pending' | pending, approved, rejected, processed |
| reviewed_by | UUID | FK → users, NULL | Admin duyệt |
| reviewed_at | TIMESTAMP | NULL | Ngày duyệt |
| rejection_reason | TEXT | NULL | Lý do từ chối |
| refunded_at | TIMESTAMP | NULL | Ngày hoàn tiền |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

---

### MODULE 12: Notification

#### 12.1 notifications

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | Người nhận |
| template_id | INT | FK → notification_templates, NULL | Template |
| title | VARCHAR(255) | NOT NULL | Tiêu đề |
| body | TEXT | NOT NULL | Nội dung |
| data | JSONB | DEFAULT '{}' | Dữ liệu bổ sung |
| channel | ENUM | NOT NULL, DEFAULT 'in_app' | email, in_app, both |
| is_read | BOOLEAN | DEFAULT false | Đã đọc |
| read_at | TIMESTAMP | NULL | Ngày đọc |
| email_sent_at | TIMESTAMP | NULL | Ngày gửi email |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

#### 12.2 notification_templates

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | SERIAL | PK | Khóa chính |
| code | VARCHAR(100) | NOT NULL, UNIQUE | Mã template |
| title_template | TEXT | NOT NULL | Template tiêu đề |
| body_template | TEXT | NOT NULL | Template nội dung |
| channel | ENUM | NOT NULL, DEFAULT 'both' | email, in_app, both |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

#### 12.3 notification_preferences

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NOT NULL | User |
| template_code | VARCHAR(100) | NOT NULL | Mã template |
| email_enabled | BOOLEAN | DEFAULT true | Nhận email |
| in_app_enabled | BOOLEAN | DEFAULT true | Nhận in-app |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

**Unique constraint:** (user_id, template_code)

---

### MODULE 13: System

#### 13.1 system_configs

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | SERIAL | PK | Khóa chính |
| key | VARCHAR(100) | NOT NULL, UNIQUE | Khóa cấu hình |
| value | TEXT | NOT NULL | Giá trị |
| value_type | VARCHAR(20) | NOT NULL, DEFAULT 'string' | string, int, decimal, boolean, json |
| description | TEXT | NULL | Mô tả |
| updated_by | UUID | FK → users, NULL | Người cập nhật |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày cập nhật |

#### 13.2 config_audit_logs

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| config_key | VARCHAR(100) | NOT NULL | Khóa cấu hình |
| old_value | TEXT | NULL | Giá trị cũ |
| new_value | TEXT | NOT NULL | Giá trị mới |
| changed_by | UUID | FK → users, NOT NULL | Người thay đổi |
| changed_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày thay đổi |

#### 13.3 audit_logs

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|-----|--------------|-----------|-------|
| id | UUID | PK | Khóa chính |
| user_id | UUID | FK → users, NULL | User thực hiện |
| action | VARCHAR(100) | NOT NULL | Hành động |
| entity_type | VARCHAR(100) | NOT NULL | Loại entity |
| entity_id | UUID | NOT NULL | ID entity |
| old_values | JSONB | NULL | Giá trị cũ |
| new_values | JSONB | NULL | Giá trị mới |
| ip_address | INET | NULL | Địa chỉ IP |
| user_agent | TEXT | NULL | Thiết bị |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Ngày tạo |

---

## Các giá trị ENUM

| ENUM | Giá trị | Mô tả |
|------|---------|-------|
| **user_status** | active | Đang hoạt động |
| | inactive | Không hoạt động |
| | locked | Bị khóa |
| | pending_verification | Chờ xác thực email |
| **course_status** | draft | Bản nháp |
| | pending_review | Chờ thẩm định |
| | needs_revision | Cần chỉnh sửa |
| | rejected | Bị từ chối |
| | published | Đã xuất bản |
| | suspended | Bị gỡ |
| **course_level** | beginner | Cơ bản |
| | intermediate | Trung cấp |
| | advanced | Nâng cao |
| **lesson_type** | video | Video |
| | article | Bài viết |
| | quiz | Bài kiểm tra |
| | document | Tài liệu |
| **order_status** | pending | Chờ thanh toán |
| | completed | Hoàn thành |
| | failed | Thất bại |
| | cancelled | Đã hủy |
| | refunded | Đã hoàn tiền |
| **payout_status** | pending | Chờ xử lý |
| | processing | Đang xử lý |
| | completed | Hoàn thành |
| | failed | Thất bại |
| **review_decision** | approved | Duyệt |
| | rejected | Từ chối |
| | needs_revision | Yêu cầu chỉnh sửa |
| **discount_type** | percentage | Giảm theo % |
| | fixed_amount | Giảm số tiền cố định |
| **refund_status** | pending | Chờ xử lý |
| | approved | Đã duyệt |
| | rejected | Từ chối |
| | processed | Đã hoàn tiền |
| **instructor_app_status** | pending | Chờ duyệt |
| | approved | Đã duyệt |
| | rejected | Từ chối |
| **notification_channel** | email | Email |
| | in_app | Trong ứng dụng |
| | both | Cả hai |

---

## Cấu hình hệ thống mặc định

| Key | Giá trị | Kiểu | Mô tả |
|-----|---------|------|-------|
| platform_fee_percent | 30 | decimal | Phí nền tảng (%) |
| refund_period_days | 7 | int | Thời hạn hoàn tiền (ngày) |
| refund_max_progress_percent | 20 | int | Tiến độ tối đa được hoàn tiền (%) |
| min_payout_amount | 500000 | decimal | Số tiền rút tối thiểu (VNĐ) |
| max_login_attempts | 5 | int | Số lần đăng nhập sai tối đa |
| account_lock_minutes | 15 | int | Thời gian khóa tài khoản (phút) |
| jwt_access_token_minutes | 15 | int | Thời hạn access token (phút) |
| jwt_refresh_token_days | 7 | int | Thời hạn refresh token (ngày) |
| order_expiry_minutes | 30 | int | Thời gian hết hạn đơn hàng (phút) |

---

## Mô tả chức năng từng bảng theo luồng thực tế

### LUỒNG 1: ĐĂNG KÝ & XÁC THỰC NGƯỜI DÙNG

#### 1. `users` - Trung tâm của hệ thống
**Chức năng:** Lưu trữ thông tin cơ bản của tất cả người dùng trong hệ thống (Learner, Instructor, Reviewer, Admin).

**Luồng thực tế:**
1. **Đăng ký tài khoản:** User điền form đăng ký → tạo record với `status = 'pending_verification'` → gửi email xác thực
2. **Xác thực email:** User click link → cập nhật `email_verified_at` và `status = 'active'`
3. **Đăng nhập:** Kiểm tra `email`, `password_hash`, `status` → nếu sai 5 lần liên tiếp (`failed_login_attempts`) → set `locked_until` khóa 15 phút
4. **Cập nhật profile:** User chỉnh sửa `full_name`, `avatar_url`, `bio`, `phone`, `social_links`
5. **Soft delete:** Khi xóa tài khoản → set `deleted_at` thay vì xóa hẳn (giữ lịch sử giao dịch)

---

#### 2. `roles` - Định nghĩa vai trò
**Chức năng:** Danh sách các vai trò cố định trong hệ thống.

**Dữ liệu mặc định:**
| id | name | description |
|----|------|-------------|
| 1 | learner | Học viên - có thể mua và học khóa học |
| 2 | instructor | Giảng viên - có thể tạo và bán khóa học |
| 3 | reviewer | Người thẩm định - kiểm duyệt chất lượng khóa học |
| 4 | admin | Quản trị viên - quản lý toàn bộ hệ thống |

**Luồng thực tế:**
- Khi user đăng ký mới → tự động gán role `learner`
- Khi đơn đăng ký instructor được duyệt → gán thêm role `instructor`
- Admin gán role `reviewer` hoặc `admin` thủ công

---

#### 3. `user_roles` - Gán vai trò cho người dùng
**Chức năng:** Bảng trung gian cho phép 1 user có nhiều role (VD: vừa là learner, vừa là instructor).

**Luồng thực tế:**
1. User đăng ký thành công → INSERT `user_roles` với `role_id = 1` (learner), `assigned_by = NULL` (tự động)
2. Admin duyệt đơn instructor → INSERT `user_roles` với `role_id = 2`, `assigned_by = admin_id`
3. Khi kiểm tra quyền: JOIN `user_roles` với `roles` để lấy danh sách role của user

---

#### 4. `oauth_accounts` - Đăng nhập bằng Google/Facebook
**Chức năng:** Liên kết tài khoản OAuth với user trong hệ thống.

**Luồng thực tế:**
1. **Đăng nhập lần đầu bằng Google:**
   - Nhận `provider_user_id` từ Google
   - Kiểm tra `oauth_accounts` → không tồn tại → tạo user mới (không cần password) → INSERT `oauth_accounts`
2. **Đăng nhập lần sau:**
   - Tìm `oauth_accounts` theo `provider` + `provider_user_id` → lấy `user_id` → đăng nhập thành công
3. **Liên kết thêm tài khoản:**
   - User đã có tài khoản email → đăng nhập Google → INSERT thêm record vào `oauth_accounts`

---

#### 5. `refresh_tokens` - Duy trì phiên đăng nhập
**Chức năng:** Lưu refresh token để cấp lại access token mới khi hết hạn (stateful JWT).

**Luồng thực tế:**
1. **Đăng nhập thành công:**
   - Tạo access token (15 phút) + refresh token (7 ngày)
   - Hash refresh token → INSERT vào `refresh_tokens` với `user_agent`, `ip_address`
2. **Làm mới token:**
   - Access token hết hạn → client gửi refresh token
   - Kiểm tra hash trong DB, `expires_at`, `revoked_at`
   - Hợp lệ → cấp access token mới
3. **Đăng xuất:**
   - Set `revoked_at = NOW()` → refresh token không còn hiệu lực
4. **Đăng xuất tất cả thiết bị:**
   - UPDATE tất cả record của user với `revoked_at = NOW()`

---

#### 6. `password_reset_tokens` - Quên mật khẩu
**Chức năng:** Lưu token tạm thời để đặt lại mật khẩu qua email.

**Luồng thực tế:**
1. User click "Quên mật khẩu" → nhập email
2. Tạo token ngẫu nhiên → hash → INSERT với `expires_at = NOW() + 30 phút`
3. Gửi email chứa link có token
4. User click link → kiểm tra `token_hash`, `expires_at`, `used_at` (phải NULL)
5. Hợp lệ → cho nhập mật khẩu mới → UPDATE `users.password_hash` + UPDATE `used_at = NOW()`

---

### LUỒNG 2: TRỞ THÀNH GIẢNG VIÊN

#### 7. `instructor_applications` - Đơn đăng ký làm giảng viên
**Chức năng:** Lưu đơn đăng ký của user muốn trở thành instructor để Admin xét duyệt.

**Luồng thực tế:**
1. **Learner gửi đơn:**
   - Điền form: `expertise_area`, `experience`, `qualifications`, `demo_course_url`
   - INSERT với `status = 'pending'`
2. **Admin xem danh sách đơn pending:**
   - Query `instructor_applications` WHERE `status = 'pending'`
3. **Admin duyệt:**
   - UPDATE `status = 'approved'`, `reviewed_by`, `reviewed_at`
   - INSERT `user_roles` với role instructor
   - Gửi notification chúc mừng
4. **Admin từ chối:**
   - UPDATE `status = 'rejected'`, `rejection_reason`
   - Gửi notification kèm lý do

---

#### 8. `instructor_bank_accounts` - Tài khoản ngân hàng
**Chức năng:** Lưu thông tin tài khoản ngân hàng để instructor nhận tiền.

**Luồng thực tế:**
1. **Thêm tài khoản:**
   - Instructor vào Settings → nhập `bank_name`, `account_number`, `account_holder_name`
   - Nếu là tài khoản đầu tiên → `is_primary = true`
2. **Đặt tài khoản chính:**
   - UPDATE `is_primary = false` cho tất cả TK cũ → UPDATE `is_primary = true` cho TK mới
3. **Rút tiền:**
   - Chọn tài khoản ngân hàng từ danh sách → tạo payout request

---

#### 9. `instructor_earnings` - Doanh thu từng giao dịch
**Chức năng:** Ghi nhận chi tiết doanh thu của instructor từ mỗi khóa học bán được.

**Luồng thực tế:**
1. **Học viên mua khóa học thành công:**
   - Tính toán: `gross_amount` = giá bán, `platform_fee_percent` = 30%, `net_amount` = 70% gross
   - INSERT với `is_confirmed = false`
2. **Sau 7 ngày (không có refund):**
   - Cron job UPDATE `is_confirmed = true`, `confirmed_at = NOW()`
   - Số tiền này mới được tính vào "Available Balance"
3. **Instructor tạo payout:**
   - Tổng hợp các earnings đã confirmed, chưa paid
   - UPDATE `payout_request_id` cho các record được rút

---

#### 10. `payout_requests` - Yêu cầu rút tiền
**Chức năng:** Quản lý yêu cầu rút tiền từ instructor.

**Luồng thực tế:**
1. **Tạo yêu cầu:**
   - Instructor kiểm tra balance ≥ 500,000 VNĐ
   - Chọn tài khoản ngân hàng → INSERT với `status = 'pending'`
2. **Admin xử lý:**
   - Xem danh sách pending → chuyển khoản thủ công
   - UPDATE `status = 'completed'`, `transaction_reference`, `processed_at`
   - UPDATE `instructor_earnings.is_paid = true` cho các record liên quan
3. **Thất bại:**
   - UPDATE `status = 'failed'`, `failure_reason` (VD: sai số tài khoản)

---

### LUỒNG 3: TẠO VÀ QUẢN LÝ KHÓA HỌC

#### 11. `categories` - Danh mục khóa học
**Chức năng:** Phân loại khóa học theo cây phân cấp (parent → children).

**Cấu trúc ví dụ:**
```
├── Lập trình (parent_id = NULL)
│   ├── Web Development (parent_id = 1)
│   │   ├── Frontend
│   │   └── Backend
│   └── Mobile Development (parent_id = 1)
├── Marketing (parent_id = NULL)
│   ├── Digital Marketing
│   └── Content Marketing
```

**Luồng thực tế:**
1. **Admin tạo danh mục:** INSERT với `parent_id` để xác định cấp
2. **Instructor chọn danh mục:** Hiển thị tree → chọn category cuối cùng (leaf)
3. **Hiển thị trang chủ:** Query `is_active = true` ORDER BY `sort_order`

---

#### 12. `courses` - Thông tin khóa học
**Chức năng:** Bảng chính lưu mọi thông tin về khóa học.

**Luồng thực tế:**
1. **Tạo khóa học mới:**
   - INSERT với `status = 'draft'`
   - Instructor điền: `title`, `short_description`, `full_description`, `price`, `category_id`, `level`
2. **Upload media:**
   - Upload thumbnail → UPDATE `thumbnail_url`
   - Upload preview video → UPDATE `preview_video_url`
3. **Thêm nội dung:**
   - Tạo sections và lessons (các bảng riêng)
   - Hệ thống tự động cập nhật `total_duration_minutes`, `total_lessons`
4. **Gửi thẩm định:**
   - UPDATE `status = 'pending_review'`
5. **Xuất bản:**
   - Reviewer duyệt → UPDATE `status = 'published'`, `published_at = NOW()`
6. **Thống kê (cached):**
   - Khi có enrollment mới → UPDATE `total_students++`
   - Khi có review mới → recalculate `average_rating`, `total_reviews++`

---

#### 13. `sections` - Chương/phần trong khóa học
**Chức năng:** Chia khóa học thành các phần logic để dễ theo dõi.

**Ví dụ cấu trúc:**
```
Khóa học: React.js từ Zero đến Hero
├── Section 1: Giới thiệu (sort_order: 0)
├── Section 2: JSX và Components (sort_order: 1)
├── Section 3: State và Props (sort_order: 2)
└── Section 4: Hooks (sort_order: 3)
```

**Luồng thực tế:**
1. **Tạo section:** INSERT với `course_id`, `title`, `sort_order`
2. **Sắp xếp lại:** UPDATE `sort_order` của các sections
3. **Hiển thị cho học viên:** ORDER BY `sort_order` ASC

---

#### 14. `lessons` - Bài học
**Chức năng:** Đơn vị nội dung nhỏ nhất trong khóa học.

**Các loại lesson (`lesson_type`):**
| Type | Mô tả | Nội dung chính |
|------|-------|----------------|
| video | Bài giảng video | `content_url` = link video (S3/Cloudinary) |
| article | Bài viết | `content_text` = nội dung markdown |
| quiz | Bài kiểm tra | Liên kết với `quiz_questions` |
| document | Tài liệu | `content_url` = link PDF |

**Luồng thực tế:**
1. **Tạo lesson video:**
   - Upload video → lấy URL → INSERT với `lesson_type = 'video'`, `content_url`, `duration_minutes`
2. **Bài học miễn phí (preview):**
   - Set `is_preview = true` → user chưa mua vẫn xem được
3. **Sắp xếp:** ORDER BY `sort_order` trong từng section

---

#### 15. `lesson_attachments` - Tệp đính kèm
**Chức năng:** Lưu các file bổ sung cho bài học (source code, slides, templates...).

**Luồng thực tế:**
1. **Instructor upload file:**
   - Upload lên S3 → INSERT record với `file_name`, `file_url`, `file_size_bytes`, `file_type`
2. **Học viên download:**
   - Kiểm tra enrollment → cho phép download từ `file_url`

---

### LUỒNG 4: HỆ THỐNG QUIZ

#### 16. `quiz_questions` - Câu hỏi
**Chức năng:** Lưu các câu hỏi trắc nghiệm cho bài quiz.

**Luồng thực tế:**
1. **Tạo câu hỏi:**
   - Liên kết với lesson có `lesson_type = 'quiz'`
   - INSERT: `question_text`, `explanation` (giải thích đáp án), `points`
2. **Hiển thị quiz:** ORDER BY `sort_order`

---

#### 17. `quiz_options` - Đáp án
**Chức năng:** Các lựa chọn cho mỗi câu hỏi (A, B, C, D).

**Luồng thực tế:**
1. **Tạo đáp án:** INSERT 4 options cho mỗi question
2. **Đánh dấu đáp án đúng:** Set `is_correct = true` cho 1 option
3. **Hiển thị:** ORDER BY `sort_order`, shuffle nếu cần

---

#### 18. `quiz_attempts` - Lịch sử làm quiz
**Chức năng:** Ghi lại từng câu trả lời của học viên.

**Luồng thực tế:**
1. **Học viên làm quiz:**
   - Chọn đáp án → INSERT từng record với `selected_option_id`
   - Kiểm tra `quiz_options.is_correct` → set `is_correct`
2. **Xem kết quả:**
   - Tính tổng `points` từ các câu đúng
   - Hiển thị `explanation` cho câu sai
3. **Làm lại quiz:**
   - INSERT records mới (giữ lịch sử cũ)

---

### LUỒNG 5: THẨM ĐỊNH KHÓA HỌC

#### 19. `course_reviews` - Kết quả thẩm định
**Chức năng:** Lưu đánh giá của Reviewer về chất lượng khóa học trước khi xuất bản.

**Tiêu chí chấm điểm:**
| Tiêu chí | Trọng số | Mô tả |
|----------|----------|-------|
| content_quality_score | 30% | Nội dung chính xác, hữu ích |
| structure_score | 20% | Cấu trúc logic, dễ theo dõi |
| video_quality_score | 25% | Hình ảnh, âm thanh rõ ràng |
| materials_score | 15% | Tài liệu bổ sung đầy đủ |
| metadata_score | 10% | Title, description, thumbnail |

**Luồng thực tế:**
1. **Instructor submit khóa học:**
   - UPDATE `courses.status = 'pending_review'`
2. **Reviewer được phân công:**
   - Admin assign hoặc tự claim
3. **Reviewer chấm điểm:**
   - INSERT record với 5 điểm + `overall_comment`
   - Tính `weighted_average`
4. **Quyết định:**
   - `decision = 'approved'` → UPDATE `courses.status = 'published'`
   - `decision = 'needs_revision'` → UPDATE `courses.status = 'needs_revision'` + thêm `improvement_suggestions`
   - `decision = 'rejected'` → UPDATE `courses.status = 'rejected'` + thêm `rejection_reason`

---

### LUỒNG 6: ĐĂNG KÝ HỌC VÀ THEO DÕI TIẾN ĐỘ

#### 20. `enrollments` - Đăng ký học
**Chức năng:** Ghi nhận việc học viên tham gia khóa học (mua hoặc miễn phí).

**Luồng thực tế:**
1. **Mua khóa học:**
   - Payment thành công → INSERT với `order_item_id`
   - UPDATE `courses.total_students++`
2. **Khóa học miễn phí:**
   - INSERT với `order_item_id = NULL`
3. **Theo dõi tiến độ:**
   - Mỗi lesson hoàn thành → recalculate `progress_percent`
   - Khi `progress_percent = 100` → UPDATE `completed_at`
4. **Kiểm tra quyền xem:**
   - Trước khi xem lesson → kiểm tra tồn tại enrollment

---

#### 21. `lesson_progress` - Tiến độ từng bài
**Chức năng:** Theo dõi chi tiết tiến độ học của từng bài.

**Luồng thực tế:**
1. **Xem video:**
   - Định kỳ 10s → UPDATE `video_position_seconds` (để resume)
2. **Hoàn thành bài:**
   - Video: xem ≥90% → UPDATE `is_completed = true`
   - Quiz: đạt ≥70% → `is_completed = true`
   - Article: scroll hết → `is_completed = true`
3. **Tính progress tổng:**
   - `enrollments.progress_percent = COUNT(completed) / COUNT(total) * 100`

---

#### 22. `user_notes` - Ghi chú cá nhân
**Chức năng:** Cho phép học viên ghi chú trong khi học video.

**Luồng thực tế:**
1. **Tạo ghi chú:**
   - Đang xem video → click "Add Note"
   - INSERT với `timestamp_seconds` = thời điểm hiện tại
2. **Xem lại ghi chú:**
   - Hiển thị list notes → click → video seek đến timestamp
3. **Chỉnh sửa/xóa:**
   - UPDATE `content` hoặc DELETE

---

#### 23. `certificates` - Chứng chỉ hoàn thành
**Chức năng:** Cấp chứng chỉ khi học viên hoàn thành 100% khóa học.

**Luồng thực tế:**
1. **Hoàn thành khóa học:**
   - `enrollments.progress_percent = 100`
   - Generate `certificate_number` unique (VD: LO-2024-00001)
   - Generate PDF với thông tin: tên học viên, tên khóa học, ngày cấp
   - Upload PDF → lưu `pdf_url`
   - Tạo `verification_url` để người khác xác thực
2. **Chia sẻ chứng chỉ:**
   - Học viên share link verification → ai cũng có thể kiểm tra tính hợp lệ

---

### LUỒNG 7: ĐÁNH GIÁ KHÓA HỌC

#### 24. `reviews` - Đánh giá từ học viên
**Chức năng:** Cho phép học viên đánh giá và nhận xét khóa học đã học.

**Luồng thực tế:**
1. **Viết đánh giá:**
   - Chỉ học viên đã enrolled mới được đánh giá
   - Mỗi user chỉ 1 review/course (UNIQUE constraint)
   - INSERT với `rating` (1-5 sao), `comment`
2. **Cập nhật thống kê:**
   - Recalculate `courses.average_rating`, `courses.total_reviews`
3. **Ẩn đánh giá vi phạm:**
   - Nếu `reported_count ≥ 3` → UPDATE `is_visible = false`

---

#### 25. `review_reports` - Báo cáo đánh giá vi phạm
**Chức năng:** Xử lý các đánh giá spam hoặc không phù hợp.

**Luồng thực tế:**
1. **Báo cáo:**
   - User/Instructor thấy review vi phạm → click Report
   - INSERT với `reason` (spam, offensive, fake...)
   - UPDATE `reviews.reported_count++`
2. **Admin xử lý:**
   - Xem danh sách unresolved
   - Quyết định: giữ nguyên hoặc ẩn review
   - UPDATE `is_resolved = true`, `resolution_note`

---

### LUỒNG 8: MUA KHÓA HỌC

#### 26. `orders` - Đơn hàng
**Chức năng:** Gom nhiều khóa học vào 1 đơn hàng để thanh toán.

**Luồng thực tế:**
1. **Tạo đơn:**
   - User click "Mua ngay" hoặc "Thanh toán giỏ hàng"
   - INSERT với `status = 'pending'`, `expires_at = NOW() + 30 phút`
   - Generate `order_number` (VD: ORD20240101-001)
2. **Thanh toán:**
   - Redirect đến payment gateway
   - Thành công → UPDATE `status = 'completed'`, `completed_at`
   - Thất bại → UPDATE `status = 'failed'`
3. **Hết hạn:**
   - Cron job: nếu `expires_at < NOW()` và `status = 'pending'` → UPDATE `status = 'cancelled'`

---

#### 27. `order_items` - Chi tiết đơn hàng
**Chức năng:** Lưu từng khóa học trong đơn hàng với giá tại thời điểm mua.

**Luồng thực tế:**
1. **Thêm vào đơn:**
   - INSERT với `price` = giá hiện tại của course
   - Nếu có coupon → tính `discount_amount`, `final_price`
2. **Hoàn thành mua:**
   - Tạo `enrollments` từ order_items
   - Tạo `instructor_earnings` cho instructor

---

#### 28. `payments` - Giao dịch thanh toán
**Chức năng:** Ghi nhận chi tiết từng lần thanh toán (có thể retry nhiều lần).

**Luồng thực tế:**
1. **Tạo payment:**
   - User chọn phương thức (Momo/ZaloPay/VNPay...)
   - INSERT với `status = 'pending'`
2. **Nhận callback từ gateway:**
   - Thành công → UPDATE `status = 'completed'`, `gateway_transaction_id`, `paid_at`
   - Lưu response gốc vào `gateway_response` (JSONB)
3. **Retry:**
   - Nếu failed → user có thể tạo payment mới cho cùng order

---

### LUỒNG 9: MÃ GIẢM GIÁ

#### 29. `coupons` - Mã giảm giá
**Chức năng:** Quản lý các mã khuyến mãi.

**Các loại coupon:**
| discount_type | Ví dụ | Giải thích |
|---------------|-------|------------|
| percentage | 20% | Giảm 20%, tối đa `max_discount_amount` |
| fixed_amount | 100,000đ | Giảm cố định 100k |

**Luồng thực tế:**
1. **Instructor tạo coupon riêng:**
   - INSERT với `course_id` = khóa học cụ thể, `created_by` = instructor
2. **Admin tạo coupon toàn hệ thống:**
   - INSERT với `course_id = NULL`
3. **Validate coupon:**
   - Kiểm tra: `code`, `is_active`, `starts_at ≤ NOW() ≤ expires_at`, `current_uses < max_uses`
4. **Áp dụng:**
   - Tính giảm giá → INSERT `order_items` với `coupon_id`
   - UPDATE `coupons.current_uses++`

---

#### 30. `coupon_usages` - Lịch sử sử dụng
**Chức năng:** Tracking ai đã dùng coupon nào, ngăn dùng lại.

**Luồng thực tế:**
1. **Khi thanh toán thành công:**
   - INSERT record để đánh dấu đã sử dụng
2. **Kiểm tra trước khi dùng:**
   - Query xem user đã dùng coupon này chưa

---

### LUỒNG 10: HOÀN TIỀN

#### 31. `refund_requests` - Yêu cầu hoàn tiền
**Chức năng:** Xử lý yêu cầu hoàn tiền theo chính sách (7 ngày, ≤20% progress).

**Luồng thực tế:**
1. **Học viên yêu cầu:**
   - Kiểm tra điều kiện: mua trong 7 ngày, progress ≤ 20%
   - INSERT với `progress_at_request`, `reason`
2. **Admin review:**
   - Xem `progress_at_request` để verify
   - Approve → UPDATE `status = 'approved'`
   - Hoàn tiền qua gateway → UPDATE `status = 'processed'`, `refunded_at`
3. **Hậu xử lý:**
   - Xóa enrollment hoặc đánh dấu inactive
   - Trừ `instructor_earnings` nếu chưa confirmed

---

### LUỒNG 11: THÔNG BÁO

#### 32. `notifications` - Thông báo gửi đến user
**Chức năng:** Lưu trữ mọi thông báo gửi đến người dùng.

**Các sự kiện trigger thông báo:**
| Event | Người nhận | Nội dung |
|-------|------------|----------|
| Mua khóa học | Learner | "Bạn đã đăng ký thành công khóa học X" |
| Có đánh giá mới | Instructor | "Khóa học X vừa nhận được đánh giá 5 sao" |
| Khóa học được duyệt | Instructor | "Khóa học X đã được xuất bản" |
| Yêu cầu hoàn tiền | Admin | "Có yêu cầu hoàn tiền mới cần xử lý" |

**Luồng thực tế:**
1. **Tạo notification:**
   - Sử dụng template → render `title`, `body` với variables
   - INSERT với `channel` (email, in_app, both)
2. **Gửi email (nếu cần):**
   - Queue job gửi email → UPDATE `email_sent_at`
3. **User đọc:**
   - Click notification → UPDATE `is_read = true`, `read_at`

---

#### 33. `notification_templates` - Mẫu thông báo
**Chức năng:** Định nghĩa template để tái sử dụng.

**Ví dụ template:**
| code | title_template | body_template |
|------|----------------|---------------|
| course_enrolled | Đăng ký thành công | Bạn đã đăng ký khóa học {{course_name}} |
| new_review | Đánh giá mới | {{reviewer_name}} vừa đánh giá {{rating}} sao |

---

#### 34. `notification_preferences` - Cài đặt nhận thông báo
**Chức năng:** Cho phép user tùy chỉnh loại thông báo muốn nhận.

**Luồng thực tế:**
1. **User vào Settings:**
   - Hiển thị list các loại notification
   - Toggle bật/tắt `email_enabled`, `in_app_enabled`
2. **Khi gửi notification:**
   - Check preferences → skip nếu disabled

---

### LUỒNG 12: QUẢN TRỊ HỆ THỐNG

#### 35. `system_configs` - Cấu hình hệ thống
**Chức năng:** Lưu các tham số có thể thay đổi mà không cần deploy lại code.

**Config quan trọng:**
| key | Ý nghĩa |
|-----|---------|
| platform_fee_percent | % nền tảng giữ lại từ mỗi giao dịch |
| refund_period_days | Số ngày được phép hoàn tiền |
| min_payout_amount | Số tiền tối thiểu để rút |

**Luồng thực tế:**
1. **Admin thay đổi config:**
   - UPDATE `value`, `updated_by`, `updated_at`
   - INSERT vào `config_audit_logs` để tracking

---

#### 36. `config_audit_logs` - Lịch sử thay đổi cấu hình
**Chức năng:** Ghi lại mọi thay đổi config để audit.

**Luồng thực tế:**
- Mỗi khi `system_configs` thay đổi → INSERT log với `old_value`, `new_value`, `changed_by`

---

#### 37. `audit_logs` - Nhật ký hoạt động
**Chức năng:** Ghi lại các hành động quan trọng trong hệ thống để điều tra khi cần.

**Actions được log:**
| action | entity_type | Mô tả |
|--------|-------------|-------|
| user.login | users | Đăng nhập thành công |
| user.role_changed | users | Thay đổi vai trò |
| course.published | courses | Xuất bản khóa học |
| order.completed | orders | Thanh toán thành công |
| refund.processed | refund_requests | Hoàn tiền |

**Luồng thực tế:**
1. **Ghi log:**
   - Sau mỗi action quan trọng → INSERT với `old_values`, `new_values` (JSONB diff)
   - Lưu `ip_address`, `user_agent` để điều tra
2. **Xem lịch sử:**
   - Admin filter theo `entity_type`, `action`, `user_id`, time range

---

## Tóm tắt luồng dữ liệu chính

### Luồng mua khóa học hoàn chỉnh:
```
1. User xem courses (courses)
2. Thêm vào giỏ → Áp coupon (coupons) → Tạo order (orders + order_items)
3. Chọn payment method → Tạo payment (payments)
4. Callback thành công → Update order status
5. Tạo enrollment (enrollments)
6. Tạo instructor_earnings
7. Gửi notification
```

### Luồng học và hoàn thành:
```
1. Xem lesson → Cập nhật lesson_progress
2. Làm quiz → Lưu quiz_attempts
3. Ghi chú → Lưu user_notes
4. Hoàn thành 100% → Cấp certificate
5. Viết review → Cập nhật course stats
```
