<div align="center">

# 🎧 OruMongoDB Podcast Manager  
**A multi-layered, transaction-safe, MongoDB-powered podcast subscription system built with C#, WinForms & Atlas.**

<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/csharp/csharp-original.svg" width="55"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/dotnetcore/dotnetcore-original.svg" width="55"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original.svg" width="55"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" width="55"/>
</p>

</div>

---

## 💡 Description

OruMongoDB Podcast Manager is a complete end-to-end system for fetching, parsing, organizing and storing podcast feeds using:

- **WinForms** for a responsive desktop GUI  
- **MongoDB Atlas** for cloud persistence  
- **Asynchronous operations** to keep the UI fast  
- **Repository Pattern** to abstract all data access  
- **ACID MongoDB Transactions** to guarantee atomic multi-write operations  
- **A clean 4-layer architecture** used in professional enterprise apps  

The goal of this project is to demonstrate real backend engineering concepts such as:

- Clean separation of responsibilities  
- Database-agnostic business logic  
- Transaction-safe write operations  
- Input validation & domain correctness  
- Robust RSS ingestion and caching  
- Professional multi-layer design  

---

## 🧱 Key Architectural Highlights

* **Repository Pattern:** All data access is isolated behind interfaces. Business logic never touches MongoDB code.  
* **Full Transaction Support:** Multi-collection writes (feed + episodes) execute inside `StartSessionAsync()` + `StartTransaction()`.  
* **Async Everywhere:** All DB and RSS operations are asynchronous to prevent UI freezing.  
* **4-Layer Architecture:** Clean separation between UI, Business, Infrastructure, and Domain.  
* **HTML Sanitization:** Episode descriptions are cleaned to plain text for readability.  
* **Dark Mode UI:** Custom WinForms theme with consistent styling.

---

## 🧰 Tech Stack

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/csharp/csharp-original.svg" width="45" title="C#"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/dotnetcore/dotnetcore-original.svg" width="45" title=".NET"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original.svg" width="45" title="MongoDB Atlas"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" width="45" title="WinForms"/>
</p>

---

## 🎥 Demo (YouTube Link Placeholder)

<p align="center">
  <a href="https://youtu.be/YOUR_VIDEO_LINK" target="_blank">
    <img src="images/youtube-thumbnail-placeholder.png" alt="Watch the Demo Video" width="500">
  </a>
</p>

---

## 🧩 System Architecture

┌───────────────────────────┐
│ UI │ → WinForms (Buttons, Lists, Grids)
└──────────────▲────────────┘
│
┌──────────────┴────────────┐
│ BusinessLayer │ → PoddService, CategoryService
│ (Use-cases, logic) │
└──────────────▲────────────┘
│
┌──────────────┴────────────┐
│ Infrastructure │ → Repositories, MongoConnector, Transactions
└──────────────▲────────────┘
│
┌──────────────┴────────────┐
│ Domain │ → Poddflöden, PoddAvsnitt, Kategori
└────────────────────────────┘


---

## 🖼️ Screenshots

<p align="center">
  <img src="images/screenshot-placeholder-1.png" width="800">
</p>
<p align="center"><b>Screenshot 1:</b> Main UI showing fetched podcast episodes.</p>

<p align="center">
  <img src="images/screenshot-placeholder-2.png" width="800">
</p>
<p align="center"><b>Screenshot 2:</b> Category management screen with edit + delete options.</p>

<p align="center">
  <img src="images/screenshot-placeholder-3.png" width="800">
</p>
<p align="center"><b>Screenshot 3:</b> Episode details with clean HTML description.</p>

---

## ⚙️ How to Run

### 1. Clone the project
```bash
git clone https://github.com/Nordtess/CSharp-PodcastManager
cd CSharp-PodcastManager

### 2. Configure MongoDB Atlas Connection

You can override the default connection settings via environment variables:

**Windows PowerShell:**
```bash
setx MONGODB_URI "your-atlas-connection-string"
setx MONGODB_DB  "your-database-name"

## 🚀 Core Features in Detail

### ✔️ Fetch & Parse RSS Feeds
- Downloads raw RSS XML from the provided URL  
- Parses titles, publish dates, external links, and episode descriptions  
- Cleans all HTML using the built-in **HtmlCleaner** for safe, readable text  
- Displays episodes instantly in the WinForms UI  

---

### ✔️ Save Podcast Feed + Episodes (Atomic Operation)
Saving a feed triggers a **multi-document MongoDB transaction**, ensuring nothing is partially written:

1. Insert the **Poddflöden** document  
2. Assign its generated `Id` to every episode  
3. Insert all **PoddAvsnitt** documents  
4. Execute `CommitTransactionAsync()`  

If **anything** fails:
- The system calls `AbortTransactionAsync()`
- Database remains consistent  
- No half-saved feeds or episodes  

This guarantees **ACID-safe persistence** just like in real enterprise applications.

---

### ✔️ Category Management
- Create brand new categories  
- Rename existing categories  
- Delete categories  
- Assign a category to any saved feed  
- Remove a category from a feed  
- Filter the entire feed list by selected category  

The UI updates dynamically without requiring a restart.

---

### ✔️ Asynchronous Operations Everywhere
All long-running operations run with `async/await`, including:

- RSS downloading & parsing  
- MongoDB fetch and save operations  
- Multi-document transactions  

This ensures:

- The **UI never freezes**
- Operations can run in parallel  
- Better performance and responsiveness  

---

### ✔️ Clean Repository Pattern Implementation
All database logic is abstracted behind clear interfaces:

- `IPoddflodeRepository`  
- `IPoddAvsnittRepository`  
- `ICategoryRepository`  

This makes the system:

- **Easy to test** (repositories can be mocked)  
- **Easy to maintain** (changes happen in one place)  
- **Easy to extend** (add new operations without breaking others)  
- **Database-agnostic** (MongoDB could be swapped for SQL with new repo implementations)

The BusinessLayer uses only interfaces — it never needs to know how MongoDB works internally.

---

### ✔️ Robust Validation Layer
The `PoddValidator` ensures:

- RSS URLs are valid (`http` / `https`)  
- Feed names follow length rules  
- Categories cannot be duplicated  
- A feed must be **saved** before assigning/remove categories  
- A feed cannot be saved twice  
- Category renames cannot collide with existing category names  

This protects the system from invalid data and keeps the UI resilient.

---

