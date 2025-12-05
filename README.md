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
