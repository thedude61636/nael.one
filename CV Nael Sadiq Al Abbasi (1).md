# Nael Sadiq Al Abbasi

**Location:** Baghdad, Iraq  
**Email:** [me@nael.one](mailto:me@nael.one) / [NaelAlAbbasi@gmail.com](mailto:NaelAlAbbasi@gmail.com)  
**Telegram:** [@TheDude61636](https://t.me/TheDude61636)  
**Phone:** +964 770 530 9576  
**GitHub:** [github.com/thedude61636](https://github.com/thedude61636)  
**Medium:** [medium.com/@thedude61636](https://medium.com/@thedude61636)  

---

## Executive Summary

Senior Full-Stack & Systems Engineer and Technical Consultant with **10+ years of hands-on experience** architecting, engineering, and deploying mission-critical national infrastructure, distributed backends, and high-performance client applications (Desktop, Mobile, Web). 

Currently leading national-scale fuel station automation across **12+ Iraqi governorates** and Ministry of Oil distribution tracking with **Ardh Al Yamama**, architecting biometric facial recognition and industrial UHF RFID systems with **GPalms**, and delivering full-stack and mobile solutions for commercial clients including **Ardunic** and **Iraq Mall**. 

Experienced with modern self-hosted deployment and PaaS pipelines (**Dokku**, **Dokploy**) and mobile automation (**Codemagic**). Grounded in first-principles analytical reasoning with a **BSc in Applied Physics** and **1st Place in Iraq in Google Hash Code**.

---

## Core Technical Competencies

* **Languages:** C# (.NET 8 – .NET 10), Go (Golang), Dart, Kotlin, Java, TypeScript, JavaScript, SQL, PowerShell.
* **Backend & Distributed Systems:** ASP.NET Core, Go Microservices, Entity Framework Core, WebSockets & Raw TCP Sockets, Zero-Allocation Binary Serialization (`MemoryPack`), Streaming Memory Exports (`LargeXlsx`), Docker & Docker Compose, RESTful APIs, Payload CMS.
* **Frontend & Web:** Svelte / SvelteKit, Blazor (WebAssembly & Server), Next.js, React 19, MudBlazor, Microsoft Fluent UI, TypeScript, Tailwind CSS, HTML5 / CSS3.
* **Desktop & Mobile:** WPF (XAML, MVVM, Dependency Injection, Reactive Observables, Flurl), Flutter (Riverpod, Dio, BLoC), Native Android (Kotlin, Java, Jetpack, MVVM).
* **Databases & Search:** PostgreSQL (PL/pgSQL, migrations, indexing), SQL Server, SQLite, IndexedDB, Typesense.
* **Hardware Interfacing & Protocols:** TCP/IP socket daemons for biometric camera devices (`AiFaCom`), handheld industrial UHF RFID scanner integration on rugged PDAs, physical TWAIN document scanner ingestion (`Vintasoft.Twain`), thermal barcode & QR printers, fuel dispenser automation protocols.
* **DevOps, Deployment & CI/CD:** Dokku, Dokploy, Codemagic (mobile CI/CD), Docker & Docker Compose, GitHub Actions, Git, Inno Setup, automated packaging & update pipelines, JetBrains suite, performance profiling, multi-tenant system design.

---

## Professional Experience

### **Ardh Al Yamama** — *Lead Systems Engineer (Desktop & Industrial)*
*May 2019 – Present | Baghdad, Iraq*

Lead engineer responsible for mission-critical industrial automation, station terminal software, and government petroleum distribution logistics:

* **National Fuel Station Automation Network (`Go.Station.Wpf`):**
  * Architected and deployed production WPF desktop terminal clients operating on-site in fuel stations across **all 12+ Iraqi governorates** (Baghdad, Basra, Nineveh, Najaf, Babylon, Anbar, Kirkuk, Dhi Qar, Maysan, Wasit, Muthana, Saladin).
  * Interfaced directly with retail fuel dispensers, pumps, and local peripheral hardware via reactive, resilient MVVM WPF architecture.
  * Built custom zero-downtime client update packagers and automated deployment pipelines (`UpdateSaWpf`) ensuring high availability across remote distributed stations.

* **Real-Time Provincial Supervisory Monitoring Platform (`Go-Monitor-Wpf`):**
  * Built real-time monitoring and supervision systems for provincial operations managers to monitor live dispensing volume, pump status, and station health nationwide.
  * Developed governorate-specific supervisor dashboards (`Go.Monitor.App.Babylon.Supervisor`, `Go.Monitor.App.Basra.Supervisor`, etc.) packaged via Inno Setup.

* **Executive Station Telemetry & Management Platform (Flutter Mobile & Web):**
  * Developed cross-platform Flutter applications for Mobile and Web enabling executive leadership and operations management to inspect live fuel dispensing data, station performance telemetry, and provincial distribution metrics on the go from any device.

* **National Petroleum Tanker Fleet Tracking Platform (`OPDCTankers`):**
  * Engineered the enterprise distribution and logistics system for the **Oil Products Distribution Company (OPDC) / Ministry of Oil, Iraq**, tracking oil tankers and transport waybills.
  * Developed a Blazor WebAssembly (.NET 8) client with MudBlazor, integrating physical TWAIN document scanners (`Vintasoft.Twain`) for digitizing physical manifests directly within the browser client.
  * Designed automated barcode and QR manifest generation (`PrintTankerBarcodeDialog`) and high-volume Excel reporting with ClosedXML.

---

### **GPalms** — *Senior Full-Stack & Systems Architect*
*Baghdad, Iraq*

Architected hardware-integrated enterprise systems, distributed microservices, and industrial data capture platforms:

* **Biometric Facial Recognition & Attendance Daemon (`FAttendenceReports`):**
  * Architected an enterprise biometric attendance platform communicating directly over raw TCP sockets with facial recognition camera hardware (`AiFaCom`).
  * Engineered a high-throughput **.NET 10** backend paired with a reactive **Svelte + TypeScript** web interface and Microsoft Fluent UI design system.
  * Enforced double-confirmation production safety release protocols, automated translation pipelines, and Docker containerization.

* **Industrial UHF RFID Asset Logistics & Handheld PDA Auditing (`AssetManagement`):**
  * Built enterprise asset tracking software running on rugged handheld industrial PDAs equipped with UHF RFID scanners for warehouse stocktakes.
  * Implemented an offline-first Blazor WebAssembly PWA with aggressive client-side dictionary caching for continuous auditing in underground basements without Wi-Fi.
  * Optimized memory footprints using zero-allocation binary serialization (`MemoryPack`) and low-memory streamed Excel exports (`LargeXlsx`).

* **Multi-Tenant Compound Management & Financial Ledger Platform (`compound-platform`):**
  * Engineered an enterprise property and compound operations platform managing leases, utility billing, and financial ledgers across residential and commercial compounds.
  * Built the backend in **Go** using `sqlc` and **PostgreSQL** with strict **Row-Level Security (RLS)** compound isolation and strict financial invariants (`int64` whole IQD currency, immutable audit ledgers, zero float math).
  * Developed a modern, reactive RTL Arabic-first interface using **Svelte 5 (runes)** + SvelteKit, Superforms, and Valibot.

---

### **Freelance & Technical Consulting**
*Baghdad, Iraq*

Providing technical architecture, full-stack engineering, deployment automation, and mobile development for commercial clients:

* **Iraq Mall (`iraqmallpayload` / `iraqmall`):**
  * Engineered modern headless commerce and restaurant management systems built with **Payload CMS 3**, **Next.js**, **React 19**, TypeScript, Docker, and PostgreSQL/SQLite.
  * Implemented structured schema collections, multi-tenant quotation generation, and automated media handling with Sharp.
  * Automated deployments and environments using **Dokku** and **Dokploy**.

* **Ardunic (`ardunic-flutter`):**
  * Architected and engineered the production **Ardunic** mobile application on **Flutter** (Dart 3.6+).
  * Implemented modern reactive state management using **Flutter Riverpod** (`flutter_riverpod`), network layer with **Dio**, fast search indexing with **Typesense**, camera barcode scanning, Cloudflare Turnstile bot verification, and Play Store release lifecycle.
  * Configured automated mobile CI/CD pipelines via **Codemagic** for continuous building, testing, and Play Store releases.

---

### **Codelab Bootcamp** — *Instructor & Co-Organizer*
*2016 – 2019 | Baghdad, Iraq*

* Helped more than **60 young developers** learn new skills and join the software development workforce.
* Played a crucial role in filtering, screening, and selecting bootcamp participants.
* Led logistics and hands-on curriculum design covering version control, software architecture, and modern mobile/web patterns.
* Reference: [facebook.com/CodeLabBootcamp/posts/1582845961801397](https://www.facebook.com/CodeLabBootcamp/posts/1582845961801397)

---

### **d3 Studio** — *Senior Mobile & Android Developer*
*July 2015 – 2019 | Baghdad, Iraq*

* **Google Hash Code 2018:** Led the engineering team from d3 that placed **1st in Iraq** in Google Hash Code, solving complex algorithmic optimization challenges under strict time constraints.
* Architected native Android applications using MVVM architecture, Jetpack components, Kotlin, and Java.
* Developed cross-platform mobile applications in Flutter utilizing BLoC and Provider state management patterns.
* Managed Firebase applications, permissions, release keystores, and uploading production versions to the Play Store.
* Coordinated with client stakeholders and backend API teams to design and implement robust specifications.

---

### **Dijlah Lights (Freelance)** — *Android Developer*
*February 2017 – April 2017*

* Crafted native Android UI from designer sketches to fit client specifications.
* Iterated on new features to adapt to client requirements.
* Created Google Play listings and uploaded release APK builds to Google Play.

---

## Education

### **University of Technology, Baghdad** — *BSc in Applied Physics*
*Graduated 2017*

* Education focused on electromagnetic fields and solid-state physics, alongside advanced mathematics which strengthened software engineering and algorithmic problem-solving foundations.

---

## Courses, Fellowships & Community Leadership

### **UNITAR Empowering Social Entrepreneurs and Youth Leaders for Iraq Programme** — *Fellowship programme offered by Hiroshima, Japan*
*2019*

* Covered Marketing, Communication, Growth Strategies, Networking, Funding Strategies, Budgets, and preparing Pitch Decks, as part of the revamped entrepreneurship and social entrepreneurship modules.
* Learnings were augmented by study tours in Hiroshima, Japan, cultural experiences, and meetings with Hibakusha and local resource people. Study tours to Hiroshima Peace Memorial Park and Peace Memorial Museum were covered by NHK.
* References:
  * [facebook.com/UNITAR.Hiroshima/posts/3008830909145444](https://www.facebook.com/UNITAR.Hiroshima/posts/3008830909145444)
  * [facebook.com/UNITAR.Hiroshima/posts/3967825403245985](https://www.facebook.com/UNITAR.Hiroshima/posts/3967825403245985)
  * [facebook.com/UNITAR.Hiroshima/posts/3089452031083331](https://www.facebook.com/UNITAR.Hiroshima/posts/3089452031083331)

### **Fikraspace** — *Core Member & Event Coordinator*
*July 2013 – August 2017*

* Fikraspace was an initiative to bring co-working and DIY culture to Iraq, successfully transitioning into a software development bootcamp community.
* Core member responsible for managing community outreach, coordinating events, and organizing developer meetups.
* Reference: [facebook.com/FikraSpace/photos/a.115616478602546/274469562717236](https://www.facebook.com/FikraSpace/photos/a.115616478602546/274469562717236)

### **GDG Baghdad (Google Developer Group)** — *Coordinator & Speaker*
*November 2017 – September 2020*

* Coordinated developer community events, workshops, and tech conferences for developers interested in Google technology across Baghdad.
* Responsible for event logistics, technical demonstrations, and coordinating DevFest gatherings.
* Reference: [facebook.com/GDGBaghdad/posts/434520823817877](https://www.facebook.com/GDGBaghdad/posts/434520823817877)

---

## Key Awards & Publications

* **1st Place in Iraq — Google Hash Code 2018:** Competitive algorithmic programming contest (with d3 engineering team).
* **Technical Writing:** Author of software engineering articles on Medium ([@thedude61636](https://medium.com/@thedude61636)), covering Flutter state management, architectural patterns, and production engineering practices.

---

## Languages

* **Arabic:** Native, with understanding of local regional dialects.
* **English:** Fluent professional proficiency.
