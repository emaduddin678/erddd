# erddd

%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'arial'}}}%%
erDiagram
    USERS ||--o{ PROPERTIES : "owns"
    USERS ||--o{ BOOKINGS : "tenant"
    USERS ||--o{ BOOKINGS : "owner"
    USERS ||--o{ PROPERTY_VISITS : "tenant"
    USERS ||--o{ PROPERTY_VISITS : "owner"
    USERS ||--o{ PROPERTY_LIKES : "likes"
    
    PROPERTIES ||--o{ BOOKINGS : "has"
    PROPERTIES ||--o{ PROPERTY_VISITS : "has"
    PROPERTIES ||--o{ PROPERTY_LIKES : "has"
    
    USERS {
        ObjectId id "PRIMARY KEY"
        string email "UNIQUE"
        string firstName
        string lastName
        string password
        string phoneNumber
        string nidNumber
        string profileImage
        boolean isTenant
        boolean isOwner
        boolean isAdmin
        object presentAddress
        object permanentAddress
    }
    
    PROPERTIES {
        ObjectId id "PRIMARY KEY"
        string propertyId "UNIQUE"
        string title
        string description
        number propertyType
        number category
        number price
        number propertySize
        number bedrooms
        number bathrooms
        ObjectId owner "FOREIGN KEY"
        object address
        array amenities
        array images
        boolean isActive
        boolean isApproved
    }
    
    BOOKINGS {
        ObjectId id "PRIMARY KEY"
        string bookingId "UNIQUE"
        ObjectId propertyId "FK"
        ObjectId tenantId "FK"
        ObjectId ownerId "FK"
        date moveInDate
        number monthlyRent
        number securityDeposit
        number totalAmount
        string status
        date requestedAt
        date acceptedAt
    }
    
    PROPERTY_VISITS {
        ObjectId id "PRIMARY KEY"
        ObjectId propertyId "FK"
        ObjectId tenantId "FK"
        ObjectId ownerId "FK"
        date visitDate
        string visitTime
        string status
        string notes
    }
    
    PROPERTY_LIKES {
        ObjectId id "PRIMARY KEY"
        ObjectId userId "FK"
        ObjectId propertyId "FK"
        date likedAt
    }


    %%{init: {'theme':'base', 'themeVariables': { 'fontSize':'16px'}}}%%
graph TB
    subgraph USERS["👤 USERS"]
        direction TB
        U1["🔑 _id - Primary Key<br/>━━━━━━━━━━━━━━━━<br/>📧 email - UNIQUE<br/>👤 firstName<br/>👤 lastName<br/>🔒 password<br/>📱 phoneNumber<br/>🆔 nidNumber<br/>📷 profileImage<br/>🏠 presentAddress<br/>🏡 permanentAddress<br/>💼 occupation<br/>✅ isTenant<br/>✅ isOwner<br/>👑 isAdmin"]
    end
    
    subgraph PROPERTIES["🏢 PROPERTIES"]
        direction TB
        P1["🔑 _id - Primary Key<br/>━━━━━━━━━━━━━━━━<br/>🏷️ propertyId - UNIQUE<br/>📝 title<br/>📄 description<br/>🏘️ propertyType<br/>📊 category<br/>💰 price<br/>📏 propertySize<br/>🛏️ bedrooms<br/>🚿 bathrooms<br/>🏢 floorNumber<br/>📍 address<br/>🎨 amenities<br/>📷 images<br/>👤 owner FK<br/>✅ isActive<br/>✅ isApproved"]
    end
    
    subgraph BOOKINGS["📅 BOOKINGS"]
        direction TB
        B1["🔑 _id - Primary Key<br/>━━━━━━━━━━━━━━━━<br/>🏷️ bookingId - UNIQUE<br/>🏢 propertyId FK<br/>👤 tenantId FK<br/>👤 ownerId FK<br/>📅 moveInDate<br/>⏱️ rentalPeriod<br/>💰 monthlyRent<br/>💵 securityDeposit<br/>💳 totalAmount<br/>📊 status<br/>📝 specialTerms<br/>🕐 requestedAt<br/>✅ acceptedAt"]
    end
    
    subgraph VISITS["👁️ PROPERTY_VISITS"]
        direction TB
        V1["🔑 _id - Primary Key<br/>━━━━━━━━━━━━━━━━<br/>🏢 propertyId FK<br/>👤 tenantId FK<br/>👤 ownerId FK<br/>📅 visitDate<br/>⏰ visitTime<br/>📊 status<br/>📝 notes"]
    end
    
    subgraph LIKES["❤️ PROPERTY_LIKES"]
        direction TB
        L1["🔑 _id - Primary Key<br/>━━━━━━━━━━━━━━━━<br/>👤 userId FK<br/>🏢 propertyId FK<br/>📅 likedAt"]
    end
    
    USERS -->|owner| PROPERTIES
    USERS -->|tenantId| BOOKINGS
    USERS -->|ownerId| BOOKINGS
    USERS -->|tenantId| VISITS
    USERS -->|ownerId| VISITS
    USERS -->|userId| LIKES
    
    PROPERTIES -->|propertyId| BOOKINGS
    PROPERTIES -->|propertyId| VISITS
    PROPERTIES -->|propertyId| LIKES
    
    style USERS fill:#e3f2fd,stroke:#1976d2,stroke-width:4px
    style PROPERTIES fill:#f3e5f5,stroke:#7b1fa2,stroke-width:4px
    style BOOKINGS fill:#fff3e0,stroke:#f57c00,stroke-width:4px
    style VISITS fill:#e8f5e9,stroke:#388e3c,stroke-width:4px
    style LIKES fill:#fce4ec,stroke:#c2185b,stroke-width:4px
