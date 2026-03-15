# Project Structure - LearnOw Platform

## Package by Feature/Domain Architecture

Cấu trúc này tổ chức code theo **chức năng nghiệp vụ** thay vì theo layer kỹ thuật, giúp:
- Dễ hiểu business logic
- Dễ maintain và scale
- Mỗi feature độc lập, dễ test
- Phù hợp với Microservices trong tương lai

---

## Cấu trúc tổng quan

```
src/main/java/com/learnow/
├── LearnowApplication.java                 # Main entry point
│
├── common/                                  # Shared utilities & base classes
│   ├── audit/
│   │   ├── AuditableEntity.java            # Base entity với createdAt, updatedAt
│   │   └── AuditLogService.java
│   ├── config/
│   │   ├── SecurityConfig.java
│   │   ├── JpaConfig.java
│   │   ├── CorsConfig.java
│   │   ├── AsyncConfig.java
│   │   └── OpenApiConfig.java
│   ├── constants/
│   │   └── AppConstants.java
│   ├── dto/
│   │   ├── ApiResponse.java                # Generic API response wrapper
│   │   ├── PagedResponse.java              # Pagination response
│   │   └── ErrorResponse.java
│   ├── exception/
│   │   ├── GlobalExceptionHandler.java     # @ControllerAdvice
│   │   ├── ResourceNotFoundException.java
│   │   ├── BadRequestException.java
│   │   ├── UnauthorizedException.java
│   │   ├── ForbiddenException.java
│   │   └── BusinessException.java
│   ├── security/
│   │   ├── JwtTokenProvider.java
│   │   ├── JwtAuthenticationFilter.java
│   │   ├── CustomUserDetailsService.java
│   │   ├── CurrentUser.java                # @CurrentUser annotation
│   │   └── UserPrincipal.java
│   ├── util/
│   │   ├── SlugUtils.java
│   │   ├── DateTimeUtils.java
│   │   └── ValidationUtils.java
│   └── validation/
│       └── ValidPhoneNumber.java           # Custom validators
│
├── auth/                                    # MODULE 1: Authentication
│   ├── controller/
│   │   └── AuthController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── LoginRequest.java
│   │   │   ├── RegisterRequest.java
│   │   │   ├── RefreshTokenRequest.java
│   │   │   ├── ForgotPasswordRequest.java
│   │   │   └── ResetPasswordRequest.java
│   │   └── response/
│   │       ├── AuthResponse.java
│   │       └── TokenResponse.java
│   ├── entity/
│   │   ├── RefreshToken.java
│   │   └── PasswordResetToken.java
│   ├── repository/
│   │   ├── RefreshTokenRepository.java
│   │   └── PasswordResetTokenRepository.java
│   ├── service/
│   │   ├── AuthService.java
│   │   └── impl/
│   │       └── AuthServiceImpl.java
│   └── oauth/
│       ├── OAuth2AuthenticationSuccessHandler.java
│       ├── OAuth2AuthenticationFailureHandler.java
│       ├── CustomOAuth2UserService.java
│       └── OAuth2UserInfo.java
│
├── user/                                    # MODULE 1: User Management
│   ├── controller/
│   │   ├── UserController.java             # Profile management
│   │   └── AdminUserController.java        # Admin user management
│   ├── dto/
│   │   ├── request/
│   │   │   ├── UpdateProfileRequest.java
│   │   │   ├── ChangePasswordRequest.java
│   │   │   └── UpdateUserStatusRequest.java
│   │   └── response/
│   │       ├── UserResponse.java
│   │       ├── UserDetailResponse.java
│   │       └── UserSummaryResponse.java
│   ├── entity/
│   │   ├── User.java
│   │   ├── Role.java
│   │   ├── UserRole.java
│   │   ├── OAuthAccount.java
│   │   └── enums/
│   │       └── UserStatus.java
│   ├── repository/
│   │   ├── UserRepository.java
│   │   ├── RoleRepository.java
│   │   ├── UserRoleRepository.java
│   │   └── OAuthAccountRepository.java
│   ├── service/
│   │   ├── UserService.java
│   │   └── impl/
│   │       └── UserServiceImpl.java
│   └── mapper/
│       └── UserMapper.java                 # MapStruct mapper
│
├── instructor/                              # MODULE 2: Instructor Management
│   ├── controller/
│   │   ├── InstructorController.java
│   │   ├── InstructorApplicationController.java
│   │   ├── InstructorEarningsController.java
│   │   └── PayoutController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── InstructorApplicationRequest.java
│   │   │   ├── BankAccountRequest.java
│   │   │   ├── PayoutRequest.java
│   │   │   └── ReviewApplicationRequest.java
│   │   └── response/
│   │       ├── InstructorApplicationResponse.java
│   │       ├── BankAccountResponse.java
│   │       ├── EarningsSummaryResponse.java
│   │       ├── EarningsDetailResponse.java
│   │       └── PayoutResponse.java
│   ├── entity/
│   │   ├── InstructorApplication.java
│   │   ├── InstructorBankAccount.java
│   │   ├── InstructorEarnings.java
│   │   ├── PayoutRequest.java
│   │   └── enums/
│   │       ├── ApplicationStatus.java
│   │       └── PayoutStatus.java
│   ├── repository/
│   │   ├── InstructorApplicationRepository.java
│   │   ├── InstructorBankAccountRepository.java
│   │   ├── InstructorEarningsRepository.java
│   │   └── PayoutRequestRepository.java
│   ├── service/
│   │   ├── InstructorService.java
│   │   ├── InstructorApplicationService.java
│   │   ├── EarningsService.java
│   │   ├── PayoutService.java
│   │   └── impl/
│   │       ├── InstructorServiceImpl.java
│   │       ├── InstructorApplicationServiceImpl.java
│   │       ├── EarningsServiceImpl.java
│   │       └── PayoutServiceImpl.java
│   └── mapper/
│       └── InstructorMapper.java
│
├── category/                                # MODULE 3: Category
│   ├── controller/
│   │   └── CategoryController.java
│   ├── dto/
│   │   ├── request/
│   │   │   └── CategoryRequest.java
│   │   └── response/
│   │       ├── CategoryResponse.java
│   │       └── CategoryTreeResponse.java
│   ├── entity/
│   │   └── Category.java
│   ├── repository/
│   │   └── CategoryRepository.java
│   ├── service/
│   │   ├── CategoryService.java
│   │   └── impl/
│   │       └── CategoryServiceImpl.java
│   └── mapper/
│       └── CategoryMapper.java
│
├── course/                                  # MODULE 4: Course Management
│   ├── controller/
│   │   ├── CourseController.java           # Public course browsing
│   │   ├── InstructorCourseController.java # Instructor course management
│   │   └── AdminCourseController.java      # Admin course management
│   ├── dto/
│   │   ├── request/
│   │   │   ├── CreateCourseRequest.java
│   │   │   ├── UpdateCourseRequest.java
│   │   │   ├── CreateSectionRequest.java
│   │   │   ├── CreateLessonRequest.java
│   │   │   ├── UpdateLessonRequest.java
│   │   │   ├── ReorderRequest.java
│   │   │   └── CourseSearchRequest.java
│   │   └── response/
│   │       ├── CourseResponse.java
│   │       ├── CourseDetailResponse.java
│   │       ├── CourseSummaryResponse.java
│   │       ├── SectionResponse.java
│   │       ├── LessonResponse.java
│   │       └── LessonAttachmentResponse.java
│   ├── entity/
│   │   ├── Course.java
│   │   ├── Section.java
│   │   ├── Lesson.java
│   │   ├── LessonAttachment.java
│   │   └── enums/
│   │       ├── CourseStatus.java
│   │       ├── CourseLevel.java
│   │       └── LessonType.java
│   ├── repository/
│   │   ├── CourseRepository.java
│   │   ├── SectionRepository.java
│   │   ├── LessonRepository.java
│   │   └── LessonAttachmentRepository.java
│   ├── service/
│   │   ├── CourseService.java
│   │   ├── SectionService.java
│   │   ├── LessonService.java
│   │   └── impl/
│   │       ├── CourseServiceImpl.java
│   │       ├── SectionServiceImpl.java
│   │       └── LessonServiceImpl.java
│   ├── mapper/
│   │   └── CourseMapper.java
│   └── specification/
│       └── CourseSpecification.java        # Dynamic query builder
│
├── quiz/                                    # MODULE 5: Quiz
│   ├── controller/
│   │   ├── QuizController.java             # For learners
│   │   └── QuizManagementController.java   # For instructors
│   ├── dto/
│   │   ├── request/
│   │   │   ├── CreateQuestionRequest.java
│   │   │   ├── CreateOptionRequest.java
│   │   │   └── SubmitQuizRequest.java
│   │   └── response/
│   │       ├── QuizQuestionResponse.java
│   │       ├── QuizOptionResponse.java
│   │       ├── QuizResultResponse.java
│   │       └── QuizAttemptResponse.java
│   ├── entity/
│   │   ├── QuizQuestion.java
│   │   ├── QuizOption.java
│   │   └── QuizAttempt.java
│   ├── repository/
│   │   ├── QuizQuestionRepository.java
│   │   ├── QuizOptionRepository.java
│   │   └── QuizAttemptRepository.java
│   ├── service/
│   │   ├── QuizService.java
│   │   └── impl/
│   │       └── QuizServiceImpl.java
│   └── mapper/
│       └── QuizMapper.java
│
├── review/                                  # MODULE 6: Course Review (Tham dinh)
│   ├── controller/
│   │   └── CourseReviewController.java     # For reviewers
│   ├── dto/
│   │   ├── request/
│   │   │   └── SubmitReviewRequest.java
│   │   └── response/
│   │       ├── PendingCourseResponse.java
│   │       └── CourseReviewResponse.java
│   ├── entity/
│   │   ├── CourseReview.java
│   │   └── enums/
│   │       └── ReviewDecision.java
│   ├── repository/
│   │   └── CourseReviewRepository.java
│   ├── service/
│   │   ├── CourseReviewService.java
│   │   └── impl/
│   │       └── CourseReviewServiceImpl.java
│   └── mapper/
│       └── CourseReviewMapper.java
│
├── enrollment/                              # MODULE 7: Enrollment & Learning
│   ├── controller/
│   │   ├── EnrollmentController.java
│   │   ├── LearningController.java
│   │   └── CertificateController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── EnrollRequest.java
│   │   │   ├── UpdateProgressRequest.java
│   │   │   └── CreateNoteRequest.java
│   │   └── response/
│   │       ├── EnrollmentResponse.java
│   │       ├── LearningProgressResponse.java
│   │       ├── LessonProgressResponse.java
│   │       ├── UserNoteResponse.java
│   │       └── CertificateResponse.java
│   ├── entity/
│   │   ├── Enrollment.java
│   │   ├── LessonProgress.java
│   │   ├── UserNote.java
│   │   └── Certificate.java
│   ├── repository/
│   │   ├── EnrollmentRepository.java
│   │   ├── LessonProgressRepository.java
│   │   ├── UserNoteRepository.java
│   │   └── CertificateRepository.java
│   ├── service/
│   │   ├── EnrollmentService.java
│   │   ├── LearningService.java
│   │   ├── CertificateService.java
│   │   └── impl/
│   │       ├── EnrollmentServiceImpl.java
│   │       ├── LearningServiceImpl.java
│   │       └── CertificateServiceImpl.java
│   └── mapper/
│       └── EnrollmentMapper.java
│
├── rating/                                  # MODULE 8: Rating & Review (Danh gia tu hoc vien)
│   ├── controller/
│   │   ├── RatingController.java
│   │   └── ReviewReportController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── CreateRatingRequest.java
│   │   │   ├── UpdateRatingRequest.java
│   │   │   └── ReportReviewRequest.java
│   │   └── response/
│   │       ├── RatingResponse.java
│   │       ├── RatingSummaryResponse.java
│   │       └── ReviewReportResponse.java
│   ├── entity/
│   │   ├── Review.java                     # Rating from learners
│   │   └── ReviewReport.java
│   ├── repository/
│   │   ├── ReviewRepository.java
│   │   └── ReviewReportRepository.java
│   ├── service/
│   │   ├── RatingService.java
│   │   ├── ReviewReportService.java
│   │   └── impl/
│   │       ├── RatingServiceImpl.java
│   │       └── ReviewReportServiceImpl.java
│   └── mapper/
│       └── RatingMapper.java
│
├── order/                                   # MODULE 9: Order & Payment
│   ├── controller/
│   │   ├── OrderController.java
│   │   ├── CartController.java
│   │   └── PaymentController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── CreateOrderRequest.java
│   │   │   ├── AddToCartRequest.java
│   │   │   └── ProcessPaymentRequest.java
│   │   └── response/
│   │       ├── OrderResponse.java
│   │       ├── OrderDetailResponse.java
│   │       ├── OrderItemResponse.java
│   │       ├── CartResponse.java
│   │       └── PaymentResponse.java
│   ├── entity/
│   │   ├── Order.java
│   │   ├── OrderItem.java
│   │   ├── Payment.java
│   │   └── enums/
│   │       ├── OrderStatus.java
│   │       └── PaymentStatus.java
│   ├── repository/
│   │   ├── OrderRepository.java
│   │   ├── OrderItemRepository.java
│   │   └── PaymentRepository.java
│   ├── service/
│   │   ├── OrderService.java
│   │   ├── CartService.java
│   │   ├── PaymentService.java
│   │   └── impl/
│   │       ├── OrderServiceImpl.java
│   │       ├── CartServiceImpl.java
│   │       └── PaymentServiceImpl.java
│   ├── mapper/
│   │   └── OrderMapper.java
│   └── payment/
│       ├── PaymentGateway.java             # Interface
│       ├── MomoPaymentGateway.java
│       ├── VnPayPaymentGateway.java
│       └── ZaloPayPaymentGateway.java
│
├── coupon/                                  # MODULE 10: Coupon & Discount
│   ├── controller/
│   │   ├── CouponController.java
│   │   └── AdminCouponController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── CreateCouponRequest.java
│   │   │   ├── UpdateCouponRequest.java
│   │   │   └── ValidateCouponRequest.java
│   │   └── response/
│   │       ├── CouponResponse.java
│   │       └── CouponValidationResponse.java
│   ├── entity/
│   │   ├── Coupon.java
│   │   ├── CouponUsage.java
│   │   └── enums/
│   │       └── DiscountType.java
│   ├── repository/
│   │   ├── CouponRepository.java
│   │   └── CouponUsageRepository.java
│   ├── service/
│   │   ├── CouponService.java
│   │   └── impl/
│   │       └── CouponServiceImpl.java
│   └── mapper/
│       └── CouponMapper.java
│
├── refund/                                  # MODULE 11: Refund
│   ├── controller/
│   │   ├── RefundController.java           # For learners
│   │   └── AdminRefundController.java      # For admin
│   ├── dto/
│   │   ├── request/
│   │   │   ├── RefundRequest.java
│   │   │   └── ProcessRefundRequest.java
│   │   └── response/
│   │       └── RefundResponse.java
│   ├── entity/
│   │   ├── RefundRequest.java
│   │   └── enums/
│   │       └── RefundStatus.java
│   ├── repository/
│   │   └── RefundRequestRepository.java
│   ├── service/
│   │   ├── RefundService.java
│   │   └── impl/
│   │       └── RefundServiceImpl.java
│   └── mapper/
│       └── RefundMapper.java
│
├── notification/                            # MODULE 12: Notification
│   ├── controller/
│   │   └── NotificationController.java
│   ├── dto/
│   │   ├── request/
│   │   │   └── UpdatePreferencesRequest.java
│   │   └── response/
│   │       ├── NotificationResponse.java
│   │       └── NotificationPreferencesResponse.java
│   ├── entity/
│   │   ├── Notification.java
│   │   ├── NotificationTemplate.java
│   │   ├── NotificationPreferences.java
│   │   └── enums/
│   │       └── NotificationChannel.java
│   ├── repository/
│   │   ├── NotificationRepository.java
│   │   ├── NotificationTemplateRepository.java
│   │   └── NotificationPreferencesRepository.java
│   ├── service/
│   │   ├── NotificationService.java
│   │   ├── EmailService.java
│   │   └── impl/
│   │       ├── NotificationServiceImpl.java
│   │       └── EmailServiceImpl.java
│   ├── mapper/
│   │   └── NotificationMapper.java
│   └── event/
│       ├── NotificationEvent.java
│       └── NotificationEventListener.java
│
├── admin/                                   # MODULE 13: System Administration
│   ├── controller/
│   │   ├── SystemConfigController.java
│   │   ├── DashboardController.java
│   │   └── AuditLogController.java
│   ├── dto/
│   │   ├── request/
│   │   │   └── UpdateConfigRequest.java
│   │   └── response/
│   │       ├── SystemConfigResponse.java
│   │       ├── DashboardResponse.java
│   │       ├── RevenueReportResponse.java
│   │       └── AuditLogResponse.java
│   ├── entity/
│   │   ├── SystemConfig.java
│   │   ├── ConfigAuditLog.java
│   │   └── AuditLog.java
│   ├── repository/
│   │   ├── SystemConfigRepository.java
│   │   ├── ConfigAuditLogRepository.java
│   │   └── AuditLogRepository.java
│   ├── service/
│   │   ├── SystemConfigService.java
│   │   ├── DashboardService.java
│   │   ├── ReportService.java
│   │   └── impl/
│   │       ├── SystemConfigServiceImpl.java
│   │       ├── DashboardServiceImpl.java
│   │       └── ReportServiceImpl.java
│   └── mapper/
│       └── AdminMapper.java
│
└── storage/                                 # File Storage Module
    ├── controller/
    │   └── StorageController.java
    ├── dto/
    │   └── response/
    │       └── UploadResponse.java
    ├── service/
    │   ├── StorageService.java
    │   └── impl/
    │       ├── LocalStorageService.java
    │       └── S3StorageService.java
    └── config/
        └── StorageConfig.java
```

---

## Resources Structure

```
src/main/resources/
├── application.yaml                         # Main config
├── application-dev.yaml                     # Development profile
├── application-prod.yaml                    # Production profile
├── application-test.yaml                    # Test profile
├── db/
│   └── migration/                           # Flyway migrations
│       ├── V1__create_users_tables.sql
│       ├── V2__create_roles_tables.sql
│       ├── V3__create_instructor_tables.sql
│       ├── V4__create_category_tables.sql
│       ├── V5__create_course_tables.sql
│       ├── V6__create_quiz_tables.sql
│       ├── V7__create_enrollment_tables.sql
│       ├── V8__create_rating_tables.sql
│       ├── V9__create_order_tables.sql
│       ├── V10__create_coupon_tables.sql
│       ├── V11__create_refund_tables.sql
│       ├── V12__create_notification_tables.sql
│       ├── V13__create_system_tables.sql
│       └── V14__seed_initial_data.sql
├── templates/
│   └── email/                               # Email templates (Thymeleaf)
│       ├── welcome.html
│       ├── password-reset.html
│       ├── order-confirmation.html
│       └── course-approved.html
├── messages/
│   ├── messages.properties                  # Default (Vietnamese)
│   └── messages_en.properties               # English
└── static/                                  # Static files (if needed)
```

---

## Test Structure

```
src/test/java/com/learnow/
├── auth/
│   ├── controller/
│   │   └── AuthControllerTest.java
│   └── service/
│       └── AuthServiceTest.java
├── user/
│   ├── controller/
│   │   └── UserControllerTest.java
│   ├── repository/
│   │   └── UserRepositoryTest.java
│   └── service/
│       └── UserServiceTest.java
├── course/
│   ├── controller/
│   │   └── CourseControllerTest.java
│   └── service/
│       └── CourseServiceTest.java
├── order/
│   └── service/
│       └── OrderServiceTest.java
├── integration/                             # Integration tests
│   ├── AuthIntegrationTest.java
│   ├── EnrollmentIntegrationTest.java
│   └── PaymentIntegrationTest.java
└── TestConfig.java                          # Test configurations
```

---

## Chi tiet tung layer trong moi Feature

### 1. Entity Layer
```java
// course/entity/Course.java
@Entity
@Table(name = "courses")
@Getter @Setter
@NoArgsConstructor
public class Course extends AuditableEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "instructor_id", nullable = false)
    private User instructor;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "category_id", nullable = false)
    private Category category;

    @Column(nullable = false, length = 100)
    private String title;

    @Column(nullable = false, unique = true, length = 150)
    private String slug;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private CourseStatus status = CourseStatus.DRAFT;

    @OneToMany(mappedBy = "course", cascade = CascadeType.ALL, orphanRemoval = true)
    @OrderBy("sortOrder ASC")
    private List<Section> sections = new ArrayList<>();

    // ... other fields
}
```

### 2. Repository Layer
```java
// course/repository/CourseRepository.java
@Repository
public interface CourseRepository extends JpaRepository<Course, UUID>,
                                          JpaSpecificationExecutor<Course> {

    Optional<Course> findBySlug(String slug);

    List<Course> findByInstructorIdAndStatus(UUID instructorId, CourseStatus status);

    @Query("SELECT c FROM Course c WHERE c.status = :status ORDER BY c.totalStudents DESC")
    Page<Course> findPopularCourses(@Param("status") CourseStatus status, Pageable pageable);

    @Query("SELECT c FROM Course c WHERE c.category.id = :categoryId AND c.status = 'PUBLISHED'")
    Page<Course> findByCategoryId(@Param("categoryId") Integer categoryId, Pageable pageable);

    boolean existsBySlug(String slug);
}
```

### 3. Service Layer
```java
// course/service/CourseService.java
public interface CourseService {
    CourseDetailResponse createCourse(UUID instructorId, CreateCourseRequest request);
    CourseDetailResponse updateCourse(UUID courseId, UUID instructorId, UpdateCourseRequest request);
    void deleteCourse(UUID courseId, UUID instructorId);
    CourseDetailResponse getCourseById(UUID courseId);
    CourseDetailResponse getCourseBySlug(String slug);
    PagedResponse<CourseSummaryResponse> searchCourses(CourseSearchRequest request, Pageable pageable);
    void submitForReview(UUID courseId, UUID instructorId);
}

// course/service/impl/CourseServiceImpl.java
@Service
@RequiredArgsConstructor
@Transactional
public class CourseServiceImpl implements CourseService {

    private final CourseRepository courseRepository;
    private final CategoryRepository categoryRepository;
    private final CourseMapper courseMapper;
    private final SlugUtils slugUtils;

    @Override
    public CourseDetailResponse createCourse(UUID instructorId, CreateCourseRequest request) {
        // Business logic here
    }

    // ... other methods
}
```

### 4. Controller Layer
```java
// course/controller/InstructorCourseController.java
@RestController
@RequestMapping("/api/v1/instructor/courses")
@RequiredArgsConstructor
@PreAuthorize("hasRole('INSTRUCTOR')")
@Tag(name = "Instructor Course Management")
public class InstructorCourseController {

    private final CourseService courseService;

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    @Operation(summary = "Create a new course")
    public ApiResponse<CourseDetailResponse> createCourse(
            @CurrentUser UserPrincipal currentUser,
            @Valid @RequestBody CreateCourseRequest request) {
        return ApiResponse.success(
            courseService.createCourse(currentUser.getId(), request)
        );
    }

    @PutMapping("/{courseId}")
    @Operation(summary = "Update course information")
    public ApiResponse<CourseDetailResponse> updateCourse(
            @PathVariable UUID courseId,
            @CurrentUser UserPrincipal currentUser,
            @Valid @RequestBody UpdateCourseRequest request) {
        return ApiResponse.success(
            courseService.updateCourse(courseId, currentUser.getId(), request)
        );
    }

    // ... other endpoints
}
```

### 5. DTO Layer
```java
// course/dto/request/CreateCourseRequest.java
@Data
@Builder
public class CreateCourseRequest {

    @NotBlank(message = "Title is required")
    @Size(min = 10, max = 100, message = "Title must be between 10 and 100 characters")
    private String title;

    @NotBlank(message = "Short description is required")
    @Size(max = 200)
    private String shortDescription;

    @NotBlank(message = "Full description is required")
    private String fullDescription;

    @NotNull(message = "Category is required")
    private Integer categoryId;

    @NotNull(message = "Level is required")
    private CourseLevel level;

    @DecimalMin(value = "0", message = "Price must be non-negative")
    @DecimalMax(value = "10000000", message = "Price must not exceed 10,000,000")
    private BigDecimal price;

    private String language = "vi";

    private List<String> requirements;

    private List<String> learningObjectives;
}

// course/dto/response/CourseDetailResponse.java
@Data
@Builder
public class CourseDetailResponse {
    private UUID id;
    private String title;
    private String slug;
    private String shortDescription;
    private String fullDescription;
    private String thumbnailUrl;
    private String previewVideoUrl;
    private CourseLevel level;
    private BigDecimal price;
    private BigDecimal originalPrice;
    private CourseStatus status;
    private Integer totalDurationMinutes;
    private Integer totalLessons;
    private Integer totalStudents;
    private BigDecimal averageRating;
    private Integer totalReviews;
    private LocalDateTime publishedAt;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    private UserSummaryResponse instructor;
    private CategoryResponse category;
    private List<SectionResponse> sections;
    private List<String> requirements;
    private List<String> learningObjectives;
}
```

### 6. Mapper Layer (MapStruct)
```java
// course/mapper/CourseMapper.java
@Mapper(componentModel = "spring", uses = {UserMapper.class, CategoryMapper.class})
public interface CourseMapper {

    @Mapping(target = "id", ignore = true)
    @Mapping(target = "slug", ignore = true)
    @Mapping(target = "status", constant = "DRAFT")
    @Mapping(target = "instructor", ignore = true)
    @Mapping(target = "category", ignore = true)
    Course toEntity(CreateCourseRequest request);

    CourseDetailResponse toDetailResponse(Course course);

    CourseSummaryResponse toSummaryResponse(Course course);

    @BeanMapping(nullValuePropertyMappingStrategy = NullValuePropertyMappingStrategy.IGNORE)
    void updateEntityFromRequest(UpdateCourseRequest request, @MappingTarget Course course);
}
```

---

## API Endpoints Overview

### Authentication (`/api/v1/auth`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/register` | Register new user |
| POST | `/login` | Login |
| POST | `/logout` | Logout |
| POST | `/refresh-token` | Refresh access token |
| POST | `/forgot-password` | Request password reset |
| POST | `/reset-password` | Reset password |
| GET | `/oauth2/authorize/{provider}` | OAuth2 login |

### Users (`/api/v1/users`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/me` | Get current user profile |
| PUT | `/me` | Update profile |
| PUT | `/me/password` | Change password |
| PUT | `/me/avatar` | Upload avatar |

### Admin Users (`/api/v1/admin/users`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List all users |
| GET | `/{userId}` | Get user details |
| PUT | `/{userId}/status` | Update user status |
| PUT | `/{userId}/roles` | Update user roles |

### Instructor Application (`/api/v1/instructor-applications`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/` | Submit application |
| GET | `/my` | Get my application |
| GET | `/` | List pending (Admin) |
| PUT | `/{id}/approve` | Approve (Admin) |
| PUT | `/{id}/reject` | Reject (Admin) |

### Categories (`/api/v1/categories`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List all categories |
| GET | `/tree` | Get category tree |
| GET | `/{id}` | Get category |
| POST | `/` | Create (Admin) |
| PUT | `/{id}` | Update (Admin) |
| DELETE | `/{id}` | Delete (Admin) |

### Courses (`/api/v1/courses`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Search/list courses |
| GET | `/{id}` | Get course by ID |
| GET | `/slug/{slug}` | Get course by slug |
| GET | `/popular` | Get popular courses |
| GET | `/category/{categoryId}` | Get courses by category |

### Instructor Courses (`/api/v1/instructor/courses`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List my courses |
| POST | `/` | Create course |
| GET | `/{id}` | Get course details |
| PUT | `/{id}` | Update course |
| DELETE | `/{id}` | Delete course |
| POST | `/{id}/submit-review` | Submit for review |
| POST | `/{id}/sections` | Add section |
| PUT | `/{id}/sections/{sectionId}` | Update section |
| DELETE | `/{id}/sections/{sectionId}` | Delete section |
| POST | `/{id}/sections/{sectionId}/lessons` | Add lesson |

### Course Review (`/api/v1/reviewer/courses`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/pending` | List pending courses |
| GET | `/{id}` | View course for review |
| POST | `/{id}/review` | Submit review decision |

### Enrollments (`/api/v1/enrollments`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/my` | List my enrollments |
| POST | `/` | Enroll in course |
| GET | `/{id}` | Get enrollment details |

### Learning (`/api/v1/learning`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/courses/{courseId}` | Get learning progress |
| GET | `/courses/{courseId}/lessons/{lessonId}` | Get lesson content |
| POST | `/courses/{courseId}/lessons/{lessonId}/progress` | Update progress |
| POST | `/courses/{courseId}/lessons/{lessonId}/notes` | Add note |
| GET | `/courses/{courseId}/notes` | Get my notes |

### Quiz (`/api/v1/learning/courses/{courseId}/lessons/{lessonId}/quiz`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Get quiz questions |
| POST | `/submit` | Submit quiz answers |
| GET | `/attempts` | Get my attempts |

### Certificates (`/api/v1/certificates`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/my` | List my certificates |
| GET | `/{id}` | Get certificate |
| GET | `/verify/{number}` | Verify certificate |

### Ratings (`/api/v1/courses/{courseId}/ratings`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List course ratings |
| POST | `/` | Create rating |
| PUT | `/` | Update my rating |
| DELETE | `/` | Delete my rating |
| POST | `/{id}/report` | Report rating |

### Orders (`/api/v1/orders`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List my orders |
| POST | `/` | Create order |
| GET | `/{id}` | Get order details |
| POST | `/{id}/cancel` | Cancel order |

### Cart (`/api/v1/cart`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Get cart |
| POST | `/items` | Add to cart |
| DELETE | `/items/{courseId}` | Remove from cart |
| DELETE | `/` | Clear cart |

### Payments (`/api/v1/payments`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/` | Process payment |
| POST | `/callback/{gateway}` | Payment callback |
| GET | `/{id}` | Get payment status |

### Coupons (`/api/v1/coupons`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/validate` | Validate coupon |

### Instructor Coupons (`/api/v1/instructor/coupons`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List my coupons |
| POST | `/` | Create coupon |
| PUT | `/{id}` | Update coupon |
| DELETE | `/{id}` | Delete coupon |

### Refunds (`/api/v1/refunds`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/my` | List my refund requests |
| POST | `/` | Request refund |
| GET | `/{id}` | Get refund status |

### Admin Refunds (`/api/v1/admin/refunds`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List all refund requests |
| PUT | `/{id}/approve` | Approve refund |
| PUT | `/{id}/reject` | Reject refund |

### Instructor Earnings (`/api/v1/instructor/earnings`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/summary` | Get earnings summary |
| GET | `/` | List earnings |
| GET | `/balance` | Get available balance |

### Payouts (`/api/v1/instructor/payouts`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List payout requests |
| POST | `/` | Request payout |
| GET | `/{id}` | Get payout status |

### Bank Accounts (`/api/v1/instructor/bank-accounts`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List bank accounts |
| POST | `/` | Add bank account |
| PUT | `/{id}` | Update bank account |
| DELETE | `/{id}` | Delete bank account |
| PUT | `/{id}/primary` | Set as primary |

### Notifications (`/api/v1/notifications`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List notifications |
| GET | `/unread-count` | Get unread count |
| PUT | `/{id}/read` | Mark as read |
| PUT | `/read-all` | Mark all as read |
| DELETE | `/{id}` | Delete notification |
| GET | `/preferences` | Get preferences |
| PUT | `/preferences` | Update preferences |

### Admin Dashboard (`/api/v1/admin/dashboard`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Get dashboard data |
| GET | `/revenue` | Get revenue report |
| GET | `/users` | Get user statistics |

### System Config (`/api/v1/admin/configs`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List all configs |
| PUT | `/{key}` | Update config |
| GET | `/audit-logs` | Get config change logs |

### Audit Logs (`/api/v1/admin/audit-logs`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List audit logs |
| GET | `/{id}` | Get audit log detail |

---

## Dependencies (pom.xml)

```xml
<dependencies>
    <!-- Spring Boot Starters -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- OAuth2 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-oauth2-client</artifactId>
    </dependency>

    <!-- Database -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.flywaydb</groupId>
        <artifactId>flyway-core</artifactId>
    </dependency>

    <!-- JWT -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-api</artifactId>
        <version>0.12.5</version>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-impl</artifactId>
        <version>0.12.5</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-jackson</artifactId>
        <version>0.12.5</version>
        <scope>runtime</scope>
    </dependency>

    <!-- MapStruct -->
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>1.5.5.Final</version>
    </dependency>

    <!-- OpenAPI/Swagger -->
    <dependency>
        <groupId>org.springdoc</groupId>
        <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
        <version>2.3.0</version>
    </dependency>

    <!-- AWS S3 (for file storage) -->
    <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>s3</artifactId>
        <version>2.24.0</version>
    </dependency>

    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <!-- Testing -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

## So sanh Package by Layer vs Package by Feature

### Package by Layer (Truyen thong - KHONG DUNG)
```
src/main/java/com/learnow/
├── controller/
│   ├── AuthController.java
│   ├── UserController.java
│   ├── CourseController.java
│   └── ... (20+ controllers)
├── service/
│   ├── AuthService.java
│   ├── UserService.java
│   └── ... (20+ services)
├── repository/
│   └── ... (37 repositories)
├── entity/
│   └── ... (37 entities)
└── dto/
    └── ... (100+ DTOs)
```

**Nhuoc diem:**
- Kho tim file khi du an lon
- Cac file lien quan nam o nhieu folder khac nhau
- Kho biet pham vi cua tung feature

### Package by Feature (CHON CAU TRUC NAY)
```
src/main/java/com/learnow/
├── auth/           # Tat ca code lien quan Authentication
├── user/           # Tat ca code lien quan User
├── course/         # Tat ca code lien quan Course
└── ...
```

**Uu diem:**
- Tat ca code lien quan 1 feature o cung 1 noi
- De hieu business logic
- De test, maintain, scale
- De chuyen sang microservices sau nay

---

## Luu y quan trong

1. **Naming Convention:**
   - Package: lowercase, singular (user, course, order)
   - Entity: PascalCase, singular (User, Course, Order)
   - Repository: Entity + Repository (UserRepository)
   - Service: Feature + Service (CourseService)
   - Controller: Feature + Controller hoac Role + Feature + Controller

2. **Shared code:**
   - Dat trong package `common/`
   - Bao gom: config, exception, security, util, base entity

3. **Cross-feature communication:**
   - Su dung Service injection
   - Tranh circular dependency bang cach tach interface

4. **Event-driven:**
   - Su dung Spring Events cho notification
   - `ApplicationEventPublisher` de publish event
   - `@EventListener` de handle event
