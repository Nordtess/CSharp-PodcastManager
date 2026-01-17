<div align="center">

# 🎧 PodcastHub: RSS Feed Management Done Right

**Where podcast discovery meets clean architecture—minus the bloat.** A transaction-safe, MongoDB-powered desktop application built with .NET WinForms, layered architecture, and real ACID guarantees.

### <sub>📚 Developed as part of a backend engineering course at Örebro University (but I did 98% of it)</sub>

</div>

## 💡 Description

I built PodcastHub to demonstrate how to architect a production-ready desktop application using enterprise-grade patterns. It's a complete podcast subscription manager that fetches RSS feeds, parses episodes with proper HTML sanitization, organizes content by categories, and persists everything to MongoDB Atlas with full ACID transaction support—all while keeping the WinForms UI buttery smooth with asynchronous operations.

This isn't just a feed reader—it's a showcase of **Clean Architecture**, **Repository Pattern**, and **transaction-safe NoSQL operations**, proving that you can build maintainable, scalable desktop apps without sacrificing code quality or user experience.

### Key Architectural Highlights:

- **4-Layer Clean Architecture:** The project is split into distinct layers—**Domain** (entities & rules), **BusinessLayer** (services & orchestration), **Infrastructure** (MongoDB & repositories), and **UI** (WinForms presentation). Dependencies flow inward, making business logic completely database-agnostic.
- **ACID MongoDB Transactions:** All multi-document writes (saving feeds + episodes, category deletions) use MongoDB sessions with proper commit/rollback handling. No partial saves, ever.
- **Repository Pattern Abstraction:** Complete data access isolation through `IPoddflodeRepository`, `IPoddAvsnittRepository`, and `ICategoryRepository`. Business logic never touches MongoDB drivers directly.
- **Async-First Design:** RSS parsing, database operations, and all I/O run asynchronously using `async`/`await`, ensuring zero UI freezing during long-running operations.
- **RSS/Atom Feed Parsing:** Robust feed ingestion using `SyndicationFeed` with fallback content extraction and automatic HTML stripping via HtmlAgilityPack.
- **Rich Domain Model:** Podcasts (Poddflöden), episodes (PoddAvsnitt), and categories (Kategori)—all properly modeled with MongoDB BSON annotations and client-side ID generation.
- **Custom Dark Theme:** Modern WinForms UI with custom theming, placeholder text, and responsive controls for a polished user experience.

---

## 🧰 Tech Stack

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/csharp/csharp-original.svg" width="40" height="40" alt="C#" title="C# (The Language)"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/dotnetcore/dotnetcore-original.svg" width="40" height="40" alt=".NET" title=".NET (The Framework)"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original.svg" width="40" height="40" alt="MongoDB" title="MongoDB Atlas (The Cloud Database)"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" width="40" height="40" alt="WinForms" title="WinForms (The UI Stack)"/>
  <img src="https://img.icons8.com/color/48/000000/xml.png" width="40" height="40" alt="XML/RSS" title="RSS/Atom Feed Parsing"/>
</p>

---

## 🎯 Features

### 📡 RSS Feed Fetching & Parsing

- **Asynchronous Feed Fetching:** Non-blocking HTTP requests to RSS/Atom feed URLs with proper error handling (404, timeouts, malformed XML)
- **SyndicationFeed Parsing:** Automatic detection and parsing of both RSS and Atom formats using .NET's built-in feed parser
- **HTML Sanitization:** Episode descriptions are cleaned with HtmlAgilityPack—no more raw HTML tags cluttering your UI
- **Fallback Content Extraction:** Smart fallback logic for feeds with incomplete metadata (Summary → Content → ElementExtensions)

### 💾 Transaction-Safe Persistence

- **ACID Guarantees:** MongoDB transactions ensure feeds and their episodes are saved atomically—either everything commits or nothing does
- **Session-Based Operations:** All write operations use `IClientSessionHandle` with explicit `StartTransaction()`, `CommitTransactionAsync()`, and `AbortTransactionAsync()` calls
- **Rollback on Failure:** Any exception during multi-document writes triggers an automatic abort, keeping your database consistent
- **Duplicate Prevention:** Validation ensures feeds can't be saved twice—checked both in-memory and at the database level

### 📂 Category Management

- **Create Categories:** Add new categories with case-insensitive uniqueness checks
- **Rename Categories:** Update category names transactionally
- **Delete Categories:** Remove categories and automatically clear them from all associated feeds in a single transaction
- **Assign/Remove Categories:** Tag feeds with categories or remove them with proper validation

### 🔍 Feed Organization & Discovery

- **Category Filtering:** Filter saved feeds by category with instant UI updates
- **Feed Renaming:** Custom display names for feeds (independent of the original RSS title)
- **Episode Browsing:** View all episodes for a feed with title, publish date, and cleaned descriptions
- **External Links:** One-click opening of episode pages in the default browser

### 🎨 Modern Desktop UI

- **Custom Dark Theme:** Professionally themed WinForms with dark backgrounds, accent colors, and smooth borders
- **Responsive Layout:** Proper control anchoring and docking for window resizing
- **Placeholder Text:** Modern input fields with contextual hints
- **Real-Time Feedback:** Instant UI updates after every operation (fetch, save, delete, rename)

---

## 🖼️ Screenshots

<p align="center">
  <img src="./Images/Images/pod1.png" width="800">
</p>
<p align="center">
  <strong>Screenshot 1:</strong> The main dashboard showing feed list, episode viewer, and category management in a unified dark-themed interface.
</p>

<p align="center">
  <img src="./Images/Images/pod4.png" width="600">
</p>
<p align="center">
  <strong>Screenshot 2:</strong> Episode showcase with cleaned HTML descriptions, publish dates, and quick access to external links.
</p>

<p align="center">
  <img src="./Images/Images/pod2.png" height="500">
</p>
<p align="center">
  <strong>Screenshot 3:</strong> Feed management panel with rename, category assignment, and deletion controls.
</p>

<p align="center">
  <img src="./Images/Images/pod3.png" height="500">
</p>
<p align="center">
  <strong>Screenshot 4:</strong> Category manager for creating, renaming, and removing podcast categories.
</p>

---

## ⚙️ How to Run

### Prerequisites

You'll need:

- **.NET 8 SDK** (or .NET 6+ compatible)
- **Visual Studio 2022** or **Rider** (for WinForms designer support)
- **MongoDB Atlas account** (free tier works perfectly) OR local MongoDB instance

### Step 1: Clone the Repository

```bash
git clone https://github.com/Nordtess/CSharp-PodcastManager
cd CSharp-PodcastManager
```

### Step 2: Configure MongoDB Connection

Update the connection string in [MongoConnector.cs](Infrastructure/MongoConnector.cs#L33):

```csharp
var connectionString =
    Environment.GetEnvironmentVariable("MONGODB_URI") ??
    "YOUR_MONGODB_ATLAS_CONNECTION_STRING";

var databaseName =
    Environment.GetEnvironmentVariable("MONGODB_DB") ??
    "podcasthub";
```

**Option A: Environment Variables (Recommended)**

```bash
# On Windows (PowerShell)
$env:MONGODB_URI="mongodb+srv://user:password@cluster.mongodb.net/?appName=PodcastHub"
$env:MONGODB_DB="podcasthub"

# On macOS/Linux
export MONGODB_URI="mongodb+srv://user:password@cluster.mongodb.net/?appName=PodcastHub"
export MONGODB_DB="podcasthub"
```

**Option B: Hardcode (for quick testing)**

Replace the default values directly in [MongoConnector.cs](Infrastructure/MongoConnector.cs).

### Step 3: Restore Dependencies

```bash
dotnet restore
```

### Step 4: Build & Run

```bash
# Build the solution
dotnet build

# Run the WinForms UI
cd UI
dotnet run
```

Or simply open `OruMongoDB.sln` in Visual Studio and press **F5**.

---

## 🏗️ Project Structure

```
CSharp-PodcastManager/
├── Domain/                      # Core business entities
│   ├── Kategori.cs             # Category entity with BSON annotations
│   ├── Poddflöden.cs           # Podcast feed entity
│   └── PoddAvsnitt.cs          # Podcast episode entity
├── BusinessLayer/               # Application services & orchestration
│   ├── PoddService.cs          # Feed fetching, saving, and management
│   ├── CategoryService.cs      # Category CRUD with transactions
│   ├── IRssParser.cs           # RSS/Atom parsing abstraction & implementation
│   ├── ServiceFactory.cs       # Dependency injection for services
│   └── ServiceException.cs     # Custom business logic exceptions
├── Infrastructure/             # Data access & MongoDB integration
│   ├── MongoConnector.cs       # Thread-safe MongoDB singleton
│   ├── MongoRepository.cs      # Generic repository base class
│   ├── PoddflodeRepository.cs  # Feed-specific repository
│   ├── PoddAvsnittRepository.cs # Episode-specific repository
│   └── CategoryRepository.cs   # Category-specific repository
├── Core/                       # Shared utilities
│   └── Helpers/
│       └── HtmlCleaner.cs      # HTML-to-plaintext sanitization
├── UI/                         # WinForms presentation layer
│   ├── MainUiForm.cs           # Main application window & logic
│   ├── PoddValidator.cs        # Input validation layer
│   ├── UiHelpers.cs            # UI utility methods
│   └── Program.cs              # Application entry point
└── OruMongoDB.sln              # Visual Studio solution file
```

### Layer Responsibilities

| Layer              | Responsibility                                            | Dependencies           |
| ------------------ | --------------------------------------------------------- | ---------------------- |
| **Domain**         | Pure entity definitions with MongoDB attributes           | MongoDB.Bson only      |
| **BusinessLayer**  | Service orchestration, RSS parsing, validation            | Domain, Infrastructure |
| **Infrastructure** | MongoDB connectivity, repository implementations          | Domain, MongoDB.Driver |
| **Core**           | Shared utilities (HTML cleaning, helpers)                 | None                   |
| **UI**             | WinForms controls, user interactions, visual presentation | BusinessLayer, Domain  |

---

## 🧱 Architectural Deep Dive

### Repository Pattern Implementation

All database operations are isolated behind repository interfaces:

```csharp
public interface IPoddflodeRepository : IRepository<Poddflöden>
{
    Task<Poddflöden> GetByUrlAsync(string rssUrl);
    Task<Poddflöden> GetByUrlAsync(IClientSessionHandle session, string rssUrl);
    Task UpdateCategoryAsync(IClientSessionHandle session, string poddId, string categoryId);
    Task DeleteByIdAsync(IClientSessionHandle session, string id);
}
```

**Benefits:**

- Business layer (`PoddService`) never touches MongoDB code directly
- Easy to mock repositories for unit testing
- Can swap MongoDB for SQL Server without changing business logic
- Clear contracts for data access operations

### ACID Transaction Example

Here's how saving a feed + episodes works atomically:

```csharp
await _connector.RunTransactionAsync(async session =>
{
    // 1. Check if feed already exists (prevents duplicates)
    var existing = await _poddRepo.GetByUrlAsync(session, rssUrl);
    if (existing != null)
        throw new ValidationException("Feed already saved");

    // 2. Insert the feed document
    await _poddRepo.InsertAsync(session, poddflode);

    // 3. Link all episodes to the feed and insert them
    foreach (var ep in avsnittList)
        ep.feedId = poddflode.Id;

    await _avsnittRepo.InsertManyAsync(session, avsnittList);

    // 4. Commit happens automatically if no exceptions
    // 5. If ANY step fails, the entire transaction rolls back
});
```

**What This Guarantees:**

- Either the feed **and** all episodes are saved, or nothing is saved
- No orphaned episodes without a parent feed
- No feeds saved without their episodes
- Database remains consistent even during network failures or crashes

### Asynchronous Operations

Every I/O operation uses `async`/`await`:

```csharp
// RSS fetching - doesn't block the UI thread
var (feed, episodes) = await _poddService.FetchPoddFeedAsync(rssUrl);

// Database writes - keeps UI responsive during commits
await _poddService.SavePoddSubscriptionAsync(feed, episodes);

// Category loading - smooth startup experience
await LoadCategoriesAsync();
```

**Result:** The WinForms UI never freezes, even when fetching large feeds or writing hundreds of episodes to MongoDB.

---

## 🔐 Validation & Error Handling

### Input Validation Layer

`PoddValidator` enforces business rules before any database operations:

- **RSS URL Validation:** Must be valid HTTP/HTTPS URL, not empty
- **Feed Name Validation:** Max 200 characters, not empty
- **Duplicate Prevention:** Can't save the same RSS URL twice
- **State Validation:** Feed must be saved before category operations
- **Category Uniqueness:** Case-insensitive category name checking

### Exception Hierarchy

```csharp
// Custom business exceptions
public class ServiceException : Exception
{
    public ServiceException(string message) : base(message) { }
}

public class ValidationException : ServiceException
{
    public ValidationException(string message) : base(message) { }
}
```

**Error Flow:**

1. Validation fails → `ValidationException` thrown
2. UI catches exception → Shows `MessageBox` with user-friendly message
3. Transaction errors → Automatic rollback + error propagation
4. Network errors (404, timeouts) → Mapped to `ValidationException` with context

---

## 🚀 Technologies & Patterns Used

**Backend:**

- .NET 8.0 (C# 12)
- MongoDB.Driver 2.x (official MongoDB .NET driver)
- MongoDB.Bson (BSON serialization & attributes)
- System.ServiceModel.Syndication (RSS/Atom parsing)
- HtmlAgilityPack (HTML sanitization)

**Frontend:**

- Windows Forms (.NET 8)
- Custom dark theme implementation
- Asynchronous UI operations (`async`/`await` in event handlers)
- Proper control lifecycle management

**Architectural Patterns:**

- **Repository Pattern** (data access abstraction)
- **Service Layer Pattern** (business logic orchestration)
- **Dependency Injection** (via `ServiceFactory`)
- **Clean Architecture** (4-layer separation)
- **ACID Transactions** (MongoDB sessions)

**Database:**

- MongoDB Atlas (cloud-hosted)
- BSON ObjectId generation
- Multi-document transactions
- Indexed queries (filter by URL, category ID)

---

## 📊 Database Schema

### Collections

**Poddflöden** (Podcast Feeds)

```json
{
  "_id": ObjectId("..."),
  "rssUrl": "https://example.com/feed.xml",
  "displayName": "My Favorite Podcast",
  "categoryId": "507f1f77bcf86cd799439011",
  "IsSaved": true,
  "SavedAt": ISODate("2026-01-17T12:00:00Z")
}
```

**PoddAvsnitt** (Podcast Episodes)

```json
{
  "_id": ObjectId("..."),
  "feedId": "507f1f77bcf86cd799439011",
  "title": "Episode 42: The Answer",
  "description": "Clean plaintext description (HTML stripped)",
  "publishDate": "1/15/2026",
  "link": "https://example.com/episode/42"
}
```

**Kategori** (Categories)

```json
{
  "_id": ObjectId("..."),
  "Namn": "Technology"
}
```

### Relationships

- **One-to-Many:** `Poddflöden` → `PoddAvsnitt` (one feed has many episodes)
- **Optional Reference:** `Poddflöden.categoryId` → `Kategori._id` (feed can have a category)

---

## 🛠️ Key Implementation Details

### HTML Cleaning Strategy

Episode descriptions often contain raw HTML. We clean them using a two-step process:

```csharp
public static string ToPlainText(string html)
{
    // 1. Fast path: if no HTML tags, just decode entities
    if (!html.Contains('<'))
        return WebUtility.HtmlDecode(html).Trim();

    // 2. Full parse: load into HtmlAgilityPack and extract text
    var doc = new HtmlDocument();
    doc.LoadHtml(WebUtility.HtmlDecode(html));
    return WebUtility.HtmlDecode(doc.DocumentNode.InnerText).Trim();
}
```

**Result:** No more `<p>`, `<br>`, `&nbsp;`, or `&quot;` cluttering your episode descriptions.

### RSS Feed Fallback Logic

Some feeds don't follow standards perfectly. We handle this with fallback extraction:

```csharp
private static string? ExtractText(SyndicationItem item)
{
    // 1. Try Summary (RSS <description>, Atom <summary>)
    if (item.Summary is TextSyndicationContent summaryContent)
        return summaryContent.Text;

    // 2. Try Content (Atom <content>)
    if (item.Content is TextSyndicationContent contentText)
        return contentText.Text;

    // 3. Try ElementExtensions (custom XML elements)
    foreach (var ext in item.ElementExtensions)
        return ext.GetObject<XmlElement>()?.InnerText;

    // 4. Final fallback
    return "No description available.";
}
```

### Thread-Safe MongoDB Singleton

```csharp
private static readonly Lazy<MongoConnector> _instance =
    new Lazy<MongoConnector>(() => new MongoConnector());

public static MongoConnector Instance => _instance.Value;
```

**Why Lazy?** Ensures the MongoDB client is initialized exactly once, even in multi-threaded scenarios (though WinForms is single-threaded, this is a best practice).

---

## 📝 Future Enhancements (Roadmap)

- [ ] **Background Feed Refresh:** Periodic automatic updates of saved feeds
- [ ] **Download Episode Audio:** Direct MP3/M4A downloads from episode links
- [ ] **Playback Integration:** Built-in audio player with playback controls
- [ ] **Search Functionality:** Full-text search across episode titles and descriptions
- [ ] **Export/Import:** Backup feeds to OPML (standard podcast format)
- [ ] **Statistics Dashboard:** Episode counts, category breakdowns, listening time tracking
- [ ] **Notification System:** Alert users when new episodes are available
- [ ] **Multi-User Support:** User accounts with personalized feed subscriptions
- [ ] **Cloud Sync:** Sync subscriptions across multiple devices
- [ ] **Web API:** RESTful API for mobile app integration

---

## 👨‍💻 Author

**Nordtess**  
_Full-Stack Developer & Clean Architecture Enthusiast_

Connect with me:

- GitHub: [@Nordtess](https://github.com/Nordtess)
- Portfolio: [Coming Soon]

---

## 📜 License

This project is licensed under the MIT License—feel free to use it, learn from it, or build upon it!

---

## 🙏 Acknowledgments

- **Örebro University** for the backend engineering course framework
- **MongoDB** for providing excellent .NET driver documentation
- **HtmlAgilityPack** for making HTML parsing painless
- **The .NET Team** for `System.ServiceModel.Syndication` and async/await

---

<div align="center">

**Built with ❤️ and way too much coffee in Sweden 🇸🇪**

</div>
