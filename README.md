# ⚡ Project Vault — Engineering Workspace & Portfolio

**Project Vault** is a personal portfolio and engineering workspace for robotics, electronics, hardware builds, and coding projects.

---

## 🛠 Features

- **📊 Live Engineering Dashboard**: Real-time project statistics (*Working*, *In Progress*, *Needs Debugging*, *Planned*, *Starred*), *"Continue Latest Project"* hero card, and recent build activity feed.
- **📦 Component Inventory Management**: Catalog physical electronics, sensors, microcontrollers, and tools. Track stock quantities, low-stock thresholds, storage bin locations, and bi-directional project allocations.
- **🚩 Milestone Tracking**: Project milestones checklist with completion progress tracking separate from documentation completeness.
- **⚡ Structured Wiring Tables**: Inline editable pin-connection tables (`Component`, `Component Pin`, `Controller Pin`, `Notes`) with autocomplete linked to inventory.
- **🔨 Chronological Build Log**: Detailed engineering timeline logging ideas, component acquisitions, circuit milestones, debugging problems, and verified solutions.
- **🧪 Testing Checklist & Repeatability**: Track test name, expected outcome, actual outcome, and status (*Pass*, *Fail*, *Untested*). Repeat test runs without losing historical records.
- **💻 Code Version History & Visual Diff**: Named immutable code snapshots with stable version flags and a built-in line-by-line visual diff comparison viewer.
- **📄 Technical Report Generator**: Publication-ready, printable technical reports formatted for PDF saving via browser print dialog.
- **💾 Full Workspace Backup**: Full JSON export and restore with archive validation.
- **⚡ Web Serial USB Flasher**: In-browser firmware flashing for ESP32 and Arduino boards over Web Serial.
- **🌓 Dark & Light Modes**: Accessible dual themes with persistent state.

---

## 🚀 Getting Started

Project Vault is completely self-contained in a single responsive HTML application.

### Running Locally
To run locally with Web Serial API support:

```bash
# Python 3
python -m http.server 8080
```

Open [http://localhost:8080](http://localhost:8080) in any modern browser (Chrome, Edge, or Brave recommended for Web Serial flasher support).

---

## 📁 Repository Structure

```
├── index.html           # Redirect entrypoint
├── project_vault.html   # Main application (HTML5 + CSS Tokens + CodeMirror + Vanilla JS)
├── README.md            # Repository documentation
├── FIREBASE_SETUP.md    # Optional cloud sync configuration guide
└── .gitignore
```

---

## 🔒 Security & Data Safety

All project documentation, code snapshots, and inventory data are stored locally in the browser (`localStorage`). Use the **Export** button in the top navigation bar to save full backup archives of your work at any time.
