# Student Skill Exchange Diagrams

This file contains the required diagrams for the Student Skill Exchange project:

- Context Diagram
- DFD Level 0
- DFD Level 1
- System Flowchart
- Sequence Diagram
- System Architecture Diagram
- Entity Relationship Diagram
- Class Diagram

---

# 1. Context Diagram

**Figure 3.1: Context Diagram of Student Skill Exchange System**

```mermaid
flowchart TB
    System(("Student Skill Exchange System"))

    Student["Student / Learner"]
    Tutor["Tutor"]
    Cloud["Cloud Backend Services
    Firebase Auth, Firestore,
    Supabase Storage"]

    Student -->|Account details, search criteria,
    learning requests, chat messages,
    ratings and feedback| System
    System -->|Matched skills, tutor details,
    request status, messages,
    notifications and feedback| Student

    Tutor -->|Profile details, skills,
    schedules, request decisions,
    chat messages and feedback replies| System
    System -->|Skill status, booking requests,
    chat messages, ratings,
    notifications and reports| Tutor

    System -->|Authentication, data records,
    uploaded images and files| Cloud
    Cloud -->|Login status, stored records,
    file URLs and real-time updates| System
```

Note: In this context diagram, Firebase Authentication, Cloud Firestore, and Supabase Storage are grouped as Cloud Backend Services to keep the diagram readable. Their individual roles are expanded in the DFD and system architecture diagrams.

---

# 2. DFD Level 0

**Figure 3.2: DFD Level 0 of Student Skill Exchange System**

```mermaid
flowchart LR
    Student["Student / Learner"]
    Tutor["Tutor"]
    Cloud["Cloud Backend Services"]

    System["0<br/>Student Skill<br/>Exchange System"]

    Student -- "Account details<br/>Search criteria<br/>Learning request" --> System
    System -- "Matched skills<br/>Request status<br/>Messages and feedback" --> Student

    Tutor -- "Profile details<br/>Skills and schedule<br/>Request decision" --> System
    System -- "Booking request<br/>Skill status<br/>Rating and notification" --> Tutor

    System -- "Authentication<br/>Application data<br/>Uploaded files" --> Cloud
    Cloud -- "Login status<br/>Stored records<br/>File URLs" --> System

    classDef entity fill:#4A4A4A,stroke:#4A4A4A,color:#FFFFFF,font-weight:bold;
    classDef process fill:#FFFFFF,stroke:#11C5C9,stroke-width:3px,color:#333333;

    class Student,Tutor,Cloud entity;
    class System process;
```

---

# 3. DFD Level 1

**Figure 3.3: DFD Level 1 of Student Skill Exchange System**

```mermaid
flowchart LR
    Student["Student / Learner"]
    Tutor["Tutor"]
    Cloud["Cloud Backend Services"]

    P1["1.0<br/>Authenticate and<br/>Manage Profile"]
    P2["2.0<br/>List and Manage<br/>Skills"]
    P3["3.0<br/>Search and Match<br/>Skills"]
    P4["4.0<br/>Manage Learning<br/>Requests"]
    P5["5.0<br/>Manage Chat,<br/>Feedback and Notifications"]

    D1[("D1 Users")]
    D2[("D2 Skills")]
    D3[("D3 Schedules")]
    D4[("D4 Requests")]
    D5[("D5 Chats / Feedback / Notifications")]
    D6[("D6 File Storage")]

    Student -- "login / register<br/>profile details" --> P1
    Tutor -- "login / register<br/>profile details" --> P1
    P1 -- "account status<br/>profile result" --> Student
    P1 -- "account status<br/>profile result" --> Tutor
    P1 <--> D1
    P1 <--> D6
    P1 <--> Cloud

    Tutor -- "skill details<br/>manage skill action" --> P2
    P2 -- "skill status" --> Tutor
    P2 <--> D1
    P2 <--> D2
    P2 <--> D3

    Student -- "search keyword<br/>filter criteria" --> P3
    P3 -- "matched tutors<br/>skill details" --> Student
    P3 <--> D1
    P3 <--> D2
    P3 <--> D3
    P3 <--> D5

    Student -- "selected slot<br/>learning request" --> P4
    Tutor -- "accept / reject<br/>complete request" --> P4
    P4 -- "request status" --> Student
    P4 -- "booking status" --> Tutor
    P4 <--> D3
    P4 <--> D4
    P4 --> D5

    Student -- "message / file<br/>rating feedback" --> P5
    Tutor -- "message / file<br/>feedback reply" --> P5
    P5 -- "messages<br/>notifications<br/>feedback result" --> Student
    P5 -- "messages<br/>notifications<br/>ratings" --> Tutor
    P5 <--> D4
    P5 <--> D5
    P5 <--> D6

    classDef entity fill:#FFFFFF,stroke:#555555,stroke-width:1px,color:#111111;
    classDef process fill:#FFFFFF,stroke:#555555,stroke-width:1px,color:#111111,font-weight:bold;
    classDef store fill:#FFFFFF,stroke:#555555,stroke-width:1px,color:#111111;

    class Student,Tutor,Cloud entity;
    class P1,P2,P3,P4,P5 process;
    class D1,D2,D3,D4,D5,D6 store;
```

---

# 4. System Flowchart

**Figure 3.4: Flowchart of Student Skill Exchange System**

```mermaid
flowchart TD
    Start([Start])
    LoginScreen[Login / Register Screen]
    NewUser{New user?}
    Register[Register New Account]
    Validate[Validate Login with Firebase Auth]
    Valid{Valid?}
    Error[Show Error Message]
    LoadProfile[Load User Profile]
    CompleteProfile{Profile Complete?}
    UpdateProfile[Update Profile]
    MainMenu[Main Navigation]
    Action{Select Action}
    TutorAction[List Skill / Manage Schedule]
    LearnerAction[Search and Match Skills]
    SendRequest[Send Learning Request]
    TutorDecision{Tutor Accepts?}
    Rejected[Update Request as Rejected]
    Accepted[Update Request as Accepted]
    Chat[Chat / Share File]
    Completed{Session Completed?}
    Feedback[Give Rating and Feedback]
    End([End])

    Start --> LoginScreen
    LoginScreen --> NewUser
    NewUser -- Yes --> Register --> Validate
    NewUser -- No --> Validate
    Validate --> Valid
    Valid -- No --> Error --> LoginScreen
    Valid -- Yes --> LoadProfile
    LoadProfile --> CompleteProfile
    CompleteProfile -- No --> UpdateProfile --> MainMenu
    CompleteProfile -- Yes --> MainMenu
    MainMenu --> Action
    Action -- Tutor --> TutorAction --> MainMenu
    Action -- Learner --> LearnerAction --> SendRequest --> TutorDecision
    TutorDecision -- No --> Rejected --> End
    TutorDecision -- Yes --> Accepted --> Chat --> Completed
    Completed -- No --> Chat
    Completed -- Yes --> Feedback --> End

    classDef terminal fill:#FFFFFF,stroke:#333333,stroke-width:2px,color:#111111;
    classDef process fill:#FFFFFF,stroke:#333333,stroke-width:2px,color:#111111;
    classDef decision fill:#FFFFFF,stroke:#333333,stroke-width:2px,color:#111111;

    class Start,End terminal;
    class LoginScreen,Register,Validate,Error,LoadProfile,UpdateProfile,MainMenu,TutorAction,LearnerAction,SendRequest,Rejected,Accepted,Chat,Feedback process;
    class NewUser,Valid,CompleteProfile,Action,TutorDecision,Completed decision;
```

---

# 5. Sequence Diagrams

The sequence diagram is divided into smaller interaction flows.

## 5.1 Authentication Sequence Diagram

**Figure 4.1: Authentication Sequence Diagram**

```mermaid
sequenceDiagram
    actor User
    participant App as Flutter Mobile App
    participant Auth as Firebase Authentication
    participant DB as Cloud Firestore

    User->>App: Open application
    User->>App: Enter email and password
    App->>Auth: Verify credentials
    Auth-->>App: Return authentication result
    alt Login successful
        App->>DB: Retrieve user profile
        DB-->>App: Return profile data
        App-->>User: Navigate to main screen
    else Login failed
        App-->>User: Display error message
    end
```

## 5.2 Skill Search and Request Sequence Diagram

**Figure 4.2: Skill Search and Request Sequence Diagram**

```mermaid
sequenceDiagram
    actor Learner
    participant App as Flutter Mobile App
    participant DB as Cloud Firestore
    participant Notify as Notification Service

    Learner->>App: Open Discover page
    Learner->>App: Enter search keyword or filter
    App->>DB: Query skills, schedules and feedback
    DB-->>App: Return matching tutor records
    App-->>Learner: Display matching tutors

    Learner->>App: Select tutor
    App->>DB: Retrieve tutor details and available schedules
    DB-->>App: Return tutor details
    App-->>Learner: Display tutor details

    Learner->>App: Select schedule and confirm request
    App->>DB: Check learner profile completeness
    DB-->>App: Return profile status
    alt Profile complete
        App->>DB: Create learning request
        App->>DB: Update schedule status to booked
        App->>Notify: Create request notification
        Notify->>DB: Store notification
        App-->>Learner: Display request sent message
    else Profile incomplete
        App-->>Learner: Display complete profile message
    end
```

## 5.3 Request Approval Sequence Diagram

**Figure 4.3: Request Approval Sequence Diagram**

```mermaid
sequenceDiagram
    actor Tutor
    participant App as Flutter Mobile App
    participant DB as Cloud Firestore
    participant Notify as Notification Service

    Tutor->>App: Open Requests page
    App->>DB: Retrieve pending requests
    DB-->>App: Return request list
    App-->>Tutor: Display requests

    Tutor->>App: Accept or reject request
    alt Request accepted
        App->>DB: Update request status to accepted
        App->>DB: Keep schedule as booked
        App->>Notify: Create accepted notification
        Notify->>DB: Store notification
        App-->>Tutor: Display accepted status
    else Request rejected
        App->>DB: Update request status to rejected
        App->>DB: Set schedule status to available
        App->>Notify: Create rejected notification
        Notify->>DB: Store notification
        App-->>Tutor: Display rejected status
    end
```

## 5.4 Chat and File Sharing Sequence Diagram

**Figure 4.4: Chat and File Sharing Sequence Diagram**

```mermaid
sequenceDiagram
    actor UserA as Learner
    actor UserB as Tutor
    participant App as Flutter Mobile App
    participant DB as Cloud Firestore
    participant Storage as Supabase Storage
    participant Notify as Notification Service

    UserA->>App: Open chat
    App->>DB: Create or retrieve chat room
    DB-->>App: Return chat data
    UserA->>App: Send text message
    App->>DB: Store message
    App->>Notify: Create message notification
    Notify->>DB: Store notification
    DB-->>App: Return updated messages
    App-->>UserB: Display new message

    UserB->>App: Upload image or file
    App->>Storage: Upload file
    Storage-->>App: Return file URL
    App->>DB: Store file message with URL
    App->>Notify: Create file message notification
    Notify->>DB: Store notification
    App-->>UserA: Display shared file
```

## 5.5 Feedback Sequence Diagram

**Figure 4.5: Feedback Sequence Diagram**

```mermaid
sequenceDiagram
    actor Learner
    actor Tutor
    participant App as Flutter Mobile App
    participant DB as Cloud Firestore
    participant Notify as Notification Service

    Learner->>App: Mark session completed
    App->>DB: Update request and schedule status
    App->>Notify: Create completion notification
    Notify->>DB: Store notification

    Learner->>App: Give rating and feedback
    App->>DB: Store feedback
    App->>Notify: Create feedback notification
    Notify->>DB: Store notification
    App-->>Learner: Show feedback submitted message

    Tutor->>App: View feedback
    App->>DB: Retrieve feedback
    DB-->>App: Return feedback records
    App-->>Tutor: Display rating and feedback

    Tutor->>App: Reply to feedback
    App->>DB: Update feedback reply
    App->>Notify: Create reply notification
    Notify->>DB: Store notification
    App-->>Tutor: Display reply submitted message
```

---

# 6. System Architecture Diagram

**Figure 4.1: System Architecture of Student Skill Exchange**

![System Architecture of Student Skill Exchange](diagrams/system_architecture_improved.svg)

The architecture is organized into five practical layers: Users, Presentation Layer, Application Logic Layer, Service Layer, and Data Layer. The Flutter mobile app handles the user interface and feature workflows, Firebase Authentication manages identity, Cloud Firestore stores real-time application records, and Supabase Storage stores uploaded profile pictures and chat files.

---

# 7. Entity Relationship Diagram

**Figure 4.6: Entity Relationship Diagram of Student Skill Exchange System**

![Formatted Entity Relationship Diagram](diagrams/student_skill_exchange_erd_formatted.svg)

The formatted ERD above follows the database-table style used in system documentation: each entity is shown with primary keys, foreign keys, and main attributes, while relationship lines describe how users, skills, schedules, requests, chats, messages, ratings, notifications, and storage files connect.

Editable Mermaid source:

```mermaid
erDiagram
    USERS {
        string user_id PK
        string name
        string email
        string phone_number
        string profile_picture
        string bio
        string university
        string faculty
        string course
        string gender
        string year_of_study
        boolean profileCompleted
        number profileCompletionPercent
        timestamp created_at
        timestamp updated_at
    }

    SKILLS {
        string skillId PK
        string userId FK
        string userEmail
        string userName
        string skillName
        string category
        string description
        boolean isVerified
        timestamp created_at
        timestamp updated_at
    }

    SCHEDULES {
        string scheduleId PK
        string teacherId FK
        timestamp date
        string dateText
        string day
        string startTime
        string endTime
        number duration
        string mode
        string locationOrLink
        string status
        string bookedBy FK
        string skillId FK
        timestamp created_at
        timestamp updated_at
    }

    REQUESTS {
        string requestId PK
        string senderId FK
        string receiverId FK
        string skillId FK
        string scheduleId FK
        string skillName
        string scheduleDateText
        string scheduleStartTime
        string scheduleEndTime
        string scheduleMode
        string status
        string cancelledBy FK
        timestamp cancelled_at
        timestamp completed_at
        timestamp created_at
        timestamp updated_at
    }

    CHATS {
        string chatId PK
        string requestId FK
        string skillId FK
        string skillName
        string lastMessage
        timestamp lastMessageTime
        string lastSenderId FK
        string participants
        string unreadFor FK
    }

    MESSAGES {
        string messageId PK
        string chatId FK
        string senderId FK
        string receiverId FK
        string messageText
        string fileUrl
        string fileType
        string fileName
        boolean isDeleted
        boolean isEdited
        timestamp timestamp
    }

    FEEDBACKS {
        string feedbackId PK
        string requestId FK
        string skillId FK
        string teacherId FK
        string learnerId FK
        number rating
        string feedback
        string comment
        boolean isEdited
        timestamp created_at
        timestamp updated_at
    }

    NOTIFICATIONS {
        string notificationId PK
        string userId FK
        string title
        string message
        string type
        string status
        boolean clicked
        string requestId FK
        string skillId FK
        string scheduleId FK
        string senderId FK
        string receiverId FK
        timestamp created_at
    }

    STORAGE_FILES {
        string filePath PK
        string bucket
        string publicUrl
        string ownerUserId FK
        string chatId FK
        string usedBy
    }

    USERS ||--o{ SKILLS : teaches
    USERS ||--o{ SCHEDULES : creates
    USERS ||--o{ REQUESTS : sends
    USERS ||--o{ REQUESTS : receives
    USERS ||--o{ FEEDBACKS : gives
    USERS ||--o{ FEEDBACKS : receives
    USERS ||--o{ NOTIFICATIONS : receives
    USERS ||--o{ MESSAGES : sends
    USERS ||--o{ MESSAGES : receives

    SKILLS ||--o{ REQUESTS : requested_for
    SKILLS ||--o{ SCHEDULES : booked_with
    SKILLS ||--o{ FEEDBACKS : reviewed_by
    SKILLS ||--o{ CHATS : discussed_in
    SKILLS ||--o{ NOTIFICATIONS : referenced_by

    SCHEDULES ||--o{ REQUESTS : selected_for
    SCHEDULES ||--o{ NOTIFICATIONS : referenced_by

    REQUESTS ||--o| CHATS : opens
    REQUESTS ||--o{ FEEDBACKS : produces
    REQUESTS ||--o{ NOTIFICATIONS : triggers

    CHATS ||--o{ MESSAGES : contains
    CHATS ||--o{ STORAGE_FILES : has_attachments

    STORAGE_FILES ||--o{ MESSAGES : attached_to
    STORAGE_FILES }o--|| USERS : stores_profile_picture_for
```

Note: This is a logical ERD for the implemented Firestore and Supabase data model. Firestore is document-based, so several display fields such as user names, skill names, and schedule details are duplicated inside related documents to reduce repeated reads in the Flutter app.

---

# 8. Class Diagram

**Figure 4.7: Class Diagram of Student Skill Exchange System**

```mermaid
classDiagram
    direction LR

    class StatelessWidget
    class StatefulWidget
    class State

    class MyApp {
        +build(context) Widget
    }

    class AppTheme {
        +light() ThemeData
    }

    class LoginScreen {
        +createState() State
    }

    class LoginScreenState {
        +loginUser() Future~void~
        +resetPassword() Future~void~
        +showMessage(message) void
        +build(context) Widget
    }

    class RegisterScreen {
        +createState() State
    }

    class RegisterScreenState {
        +registerUser() Future~void~
        +showMessage(message) void
        +build(context) Widget
    }

    class MainNavigationScreen {
        +createState() State
    }

    class MainNavigationScreenState {
        -currentIndex int
        -pages List~Widget~
        +destinationIcon(icon, showBadge) Widget
        +build(context) Widget
    }

    class DashboardScreen {
        +logout(context) Future~void~
        +deleteSkill(context, skillId) Future~void~
        +calculateOverallRating(feedbacks) double
        +build(context) Widget
    }

    class SkillListScreen {
        +createState() State
    }

    class SkillListScreenState {
        +openFilterSheet() void
        +getNextAvailableSlot(teacherId) Future~String~
        +calculateRating(docs) double
        +build(context) Widget
    }

    class SkillDetailScreen {
        +skillId String
        +skillData Map
        +createState() State
    }

    class SkillDetailScreenState {
        +isProfileComplete() Future~bool~
        +sendRequest() Future~void~
        +sortSchedules(docs) List
        +calculateRating(docs) double
        +build(context) Widget
    }

    class AddSkillScreen {
        +createState() State
    }

    class AddSkillScreenState {
        +saveSkill() Future~void~
        +showMessage(message) void
        +build(context) Widget
    }

    class ScheduleScreen {
        +createState() State
    }

    class ScheduleScreenState {
        +addSchedule() Future~void~
        +deleteSchedule(scheduleId, status) Future~void~
        +sortSchedules(docs) List
        +statusColor(status) Color
        +build(context) Widget
    }

    class RequestScreen {
        +updateRequestStatus() Future~void~
        +cancelBooking() Future~void~
        +markCompleted() Future~void~
        +showRequestDetail() void
        +build(context) Widget
    }

    class ChatListScreen {
        +chatList() Widget
        +build(context) Widget
    }

    class ChatScreen {
        +requestId String
        +skillId String
        +receiverId String
        +receiverEmail String
        +skillName String
        +createState() State
    }

    class ChatScreenState {
        +chatId String
        +sendMessage() Future~void~
        +pickAndUploadImage(source) Future~void~
        +pickAndUploadFile() Future~void~
        +editMessage(messageId, oldText) Future~void~
        +deleteMessage(messageId) Future~void~
        +build(context) Widget
    }

    class ProfileScreen {
        +createState() State
    }

    class ProfileScreenState {
        +loadProfile() Future~void~
        +pickAndUploadImage(source) Future~void~
        +updateProfile() Future~void~
        +build(context) Widget
    }

    class RatingScreen {
        +requestId String
        +skillId String
        +teacherId String
        +skillName String
        +createState() State
    }

    class RatingScreenState {
        +loadExistingFeedback() Future~void~
        +submitOrUpdateFeedback() Future~void~
        +deleteFeedback() Future~void~
        +build(context) Widget
    }

    class FeedbackScreen {
        +skillId String
        +replyFeedback(feedbackId, reply) Future~void~
        +build(context) Widget
    }

    class NotificationScreen {
        +markAsRead(notificationId) Future~void~
        +openTargetPage() Future~void~
        +build(context) Widget
    }

    class SupabaseService {
        +uploadProfilePicture(userId, file) Future~String~
        +uploadChatFile(chatId, file, fileName) Future~String~
    }

    class NotificationService {
        +createNotification() Future~void~
    }

    class UtemData {
        +faculties List~String~
        +facultyCourses Map
        +getCourses(faculty) List~String~
    }

    MyApp --|> StatelessWidget
    DashboardScreen --|> StatelessWidget
    RequestScreen --|> StatelessWidget
    ChatListScreen --|> StatelessWidget
    FeedbackScreen --|> StatelessWidget
    NotificationScreen --|> StatelessWidget

    LoginScreen --|> StatefulWidget
    RegisterScreen --|> StatefulWidget
    MainNavigationScreen --|> StatefulWidget
    SkillListScreen --|> StatefulWidget
    SkillDetailScreen --|> StatefulWidget
    AddSkillScreen --|> StatefulWidget
    ScheduleScreen --|> StatefulWidget
    ChatScreen --|> StatefulWidget
    ProfileScreen --|> StatefulWidget
    RatingScreen --|> StatefulWidget

    LoginScreenState --|> State
    RegisterScreenState --|> State
    MainNavigationScreenState --|> State
    SkillListScreenState --|> State
    SkillDetailScreenState --|> State
    AddSkillScreenState --|> State
    ScheduleScreenState --|> State
    ChatScreenState --|> State
    ProfileScreenState --|> State
    RatingScreenState --|> State

    LoginScreen o-- LoginScreenState
    RegisterScreen o-- RegisterScreenState
    MainNavigationScreen o-- MainNavigationScreenState
    SkillListScreen o-- SkillListScreenState
    SkillDetailScreen o-- SkillDetailScreenState
    AddSkillScreen o-- AddSkillScreenState
    ScheduleScreen o-- ScheduleScreenState
    ChatScreen o-- ChatScreenState
    ProfileScreen o-- ProfileScreenState
    RatingScreen o-- RatingScreenState

    MyApp ..> LoginScreen : starts
    MyApp ..> AppTheme : uses
    LoginScreen ..> RegisterScreen : navigates
    LoginScreen ..> MainNavigationScreen : navigates

    MainNavigationScreen o-- DashboardScreen
    MainNavigationScreen o-- SkillListScreen
    MainNavigationScreen o-- RequestScreen
    MainNavigationScreen o-- ChatListScreen
    MainNavigationScreen o-- ProfileScreen

    DashboardScreen ..> AddSkillScreen : opens
    DashboardScreen ..> ScheduleScreen : opens
    DashboardScreen ..> NotificationScreen : opens

    SkillListScreenState ..> SkillDetailScreen : opens
    SkillListScreenState ..> UtemData : uses
    SkillDetailScreenState ..> NotificationService : creates alert

    RequestScreen ..> RatingScreen : opens
    RequestScreen ..> FeedbackScreen : opens
    RequestScreen ..> NotificationService : sends alert

    ChatListScreen ..> ChatScreen : opens
    ChatScreenState ..> SupabaseService : uploads files
    ChatScreenState ..> NotificationService : sends alert

    ProfileScreenState ..> SupabaseService : uploads image
    ProfileScreenState ..> UtemData : uses

    NotificationScreen ..> RequestScreen : opens
    NotificationScreen ..> ScheduleScreen : opens
    NotificationScreen ..> FeedbackScreen : opens
    NotificationScreen ..> SkillDetailScreen : opens
    NotificationScreen ..> ChatScreen : opens
```

Note: The project does not define separate model classes for Firestore documents. Therefore, this class diagram focuses on the implemented Flutter screen classes, state classes, services, utilities, inheritance, navigation flow, and service dependencies.
