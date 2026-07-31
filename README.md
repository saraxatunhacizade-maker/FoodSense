# 🍴 FoodSense — AI-Powered Smart Menu Recommendation System

An AI-driven personalized dining experience that tailors restaurant menus using real-time weather data, dietary preferences, allergen flags, and geolocation.

![Project Status](https://img.shields.io/badge/Project--Type-IT%20Business%20Analytics-blue)
![Version](https://img.shields.io/badge/Version-1.0.0-green)

---

## 📌 Business Overview & Problem Statement

### **The Challenge**
The modern dining industry suffers from key operational inefficiencies and user friction points:
* **Menu Overload & Choice Fatigue:** Standard menus feature 70–80 items without personalized filtering, extending customer decision time by 3–5×.
* **Static Menus Ignore External Context:** Dining preferences shift with weather (e.g., cold salads on hot days, hot soups during rain), yet restaurants present static menus regardless of real-time conditions.
* **Allergy Risks Left to Customers:** Restaurants rarely automate ingredient filtering, risking health incidents for allergic diners.
* **Lack of Dietary Personalization:** Vegan, Keto, or Halal diners often receive irrelevant recommendations on primary menu interfaces.

### **The Solution**
**FoodSense** bridges these gaps by providing real-time, context-aware menu recommendations. By dynamically cross-filtering user allergy profiles, dietary preferences, time of day, location, and weather conditions, FoodSense improves the customer ordering experience while mitigating restaurant no-shows and operational bottlenecks.

---

## 💡 Key Business & Functional Requirements

### 🏢 Business Requirements (BR)
* **BR-1:** The system shall dynamically generate personalized menu recommendations by evaluating real-time weather API feeds, allergy constraints, dietary preferences, order history, and GPS location.
* **BR-2:** Any menu item containing a user-declared allergen must be automatically excluded from all presentation layers (Home, Search, Recommendations).
* **BR-3:** The platform shall integrate a digital 10-stamp loyalty card system, automatically issuing a reward upon completion of 10 qualifying orders ($\ge$ 10 AZN).

### ⚙️ Functional Requirements (FR)
* **FR-1:** Support multi-method authentication (Google OAuth, 4-digit SMS OTP with a 60-second timer, and Guest Mode).
* **FR-2:** Poll external weather APIs every 15 minutes and seamlessly update recommendation grids without full-page reloads.
* **FR-3:** Automate time-of-day contextual transitions (Breakfast: 06:00–11:00, Lunch: 12:00–16:00, Dinner: 18:00–23:00) using background task runners.
* **FR-4:** Process instant search querying starting from the first character typed, handling partial matches gracefully.

### 📈 Non-Functional Requirements (NFR)
* **Performance:** Personalization engines must deliver response payloads within 3 seconds at P95 latency.
* **Security:** Sensitive PII and payment data must be encrypted using AES-256 / TLS 1.3 standards with PCI-DSS Level 1 tokenization.
* **Reliability:** System must support at least 10,000 concurrent active sessions during peak dining hours.

---

## 🤖 AI Recommendation Engine & Logic Flow

FoodSense executes a 3-priority cross-filtering matrix whenever a user accesses the menu:
[Inputs: Weather API + User Profile + Location + Time + Order History]
│
▼
┌───────────────────────┐
│ P1: Exclusion Filter  │ ➔ Hard Removal of Allergens
└───────────┬───────────┘
│
▼
┌───────────────────────┐
│ P2: Contextual Match  │ ➔ Time of Day + Weather Alignment
└───────────┬───────────┘
│
▼
┌───────────────────────┐
│ P3: Dietary Boosting  │ ➔ Score Boost for Vegan/Keto/Halal
└───────────┬───────────┘
│
▼
[ Ranked Custom Menu Output ]
### **Weather-to-Menu Mapping Logic**
* **Hot (>25°C):** Promotes light, refreshing items (Smoothies, Salads, Cold Brew, Ice Cream).
* **Rainy / Cold (<10°C):** Prioritizes comfort foods, high-calorie meals, and hot beverages (Soups, Stews, Hot Chocolate).
* **Mild (10–25°C):** Uses standard dietary profiles as the primary ranking driver.
* **Windy / Gloomy:** Promotes quick-serve, grab-and-go options for fast takeaway.

---

## 🔄 As-Is vs. To-Be Process Transformation

| Process Step | ❌ As-Is (Legacy Manual Flow) | ✅ To-Be (FoodSense Automated Flow) |
| :--- | :--- | :--- |
| **Reservation** | Phone call / manual social media message; risk of lost records. | Instant app booking with table locking and dynamic QR ticket confirmation. |
| **Table Locking** | Manual paper log; frequent double-booking issues. | Database-level table locking with temporary holding holds. |
| **Menu Browsing** | Physical menu distribution; no allergen/dietary filters. | Dynamic QR scan revealing real-time, pre-filtered digital menus. |
| **Payment** | Physical check delivery and POS terminal retrieval (15-20 min delay). | In-app one-touch digital payment with instant kitchen display notification. |

---

## 🛠️ System Architecture & Data Flow
┌────────────────────────────────────────────────────────────────────────┐
│                          FoodSense Ecosystem                           │
├───────────────────┬───────────────────────┬────────────────────────────┤
│  Frontend Mobile  │    Backend Services   │   Data & Integrations      │
│                   │                       │                            │
│  • React Native   │  • Node.js / FastAPI  │  • PostgreSQL / Redis      │
│  • QR Scanning    │  • Microservices      │  • OpenWeatherMap API      │
│  • Offline-First  │  • Two-Phase Payment  │  • Stripe / Payriff SDK    │
└───────────────────┴───────────────────────┴────────────────────────────┘


### **Key Technical Safeguards**
1. **Two-Phase Payment (Auth/Capture):** Prevents lost orders if network failures occur post-payment. Funds are captured only after the Kitchen Display System (KDS) confirms receipt.
2. **Pessimistic Database Locking:** Blocks table race conditions during peak booking hours by placing a temporary 5-minute lock when a user selects a table on the map.
3. **Edge Node Resilience:** Local edge devices (e.g., Raspberry Pi) maintain order queues on local LANs if internet connectivity drops, synchronizing automatically upon reconnection.

---

## 👥 Stakeholder Matrix (RACI)

* **Responsible (R):** Frontend & Backend Engineering Teams
* **Accountable (A):** Product Owner / IT Business Analyst
* **Consulted (C):** UI/UX Designers, QA Team
* **Informed (I):** Customer Support & Restaurant Operations

---

## 🚀 Product Roadmap

* **v1.0 (Current):** Core AI recommendation engine, weather integration, dynamic dietary/allergen filtering, digital loyalty card, QR ordering.
* **v2.0 (Planned):** AR menu previews, conversational AI ordering assistant, dynamic pricing engines, multi-language interface (AZ, EN, RU).
* **v3.0 (Long-Term):** Machine-learning menu optimization, regional expansion, voice-activated ordering (Siri/Google Assistant), carbon footprint tracking.
