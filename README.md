import { Outlet } from "react-router-dom";

export const CockpitLayout = () => {
  return (
    <div
      className="relative flex h-screen w-full overflow-hidden"
      data-testid="cockpit-layout"
    >
      {/* Sidebar */}
      <aside
        className="hidden md:flex w-80 flex-col
                   bg-zinc-900/50 backdrop-blur-2xl
                   border-r border-white/5
                   h-screen sticky top-0 z-30"
        data-testid="cockpit-sidebar"
      >
        <div className="p-6 space-y-8">
          <div>
            <p className="text-xs uppercase tracking-widest text-zinc-500 font-mono">
              System
            </p>
            <h1 className="text-xl font-semibold tracking-tight">
              E1 Cockpit
            </h1>
          </div>

          <nav className="space-y-2">
            {["Primary Channel", "Secondary Channel", "Archive"].map(
              (item) => (
                <div
                  key={item}
                  className="group flex items-center gap-4 p-3 rounded-xl
                             hover:bg-white/5 transition-colors
                             cursor-pointer border border-transparent
                             hover:border-white/5"
                  data-testid={`sidebar-item-${item}`}
                >
                  <div className="h-2 w-2 rounded-full bg-violet-500/60" />
                  <span className="text-sm text-zinc-300 group-hover:text-white">
                    {item}
                  </span>
                </div>
              )
            )}
          </nav>
        </div>
      </aside>

      {/* Main Chat Canvas */}
      <main
        className="relative flex-1 flex flex-col
                   bg-[#09090b]
                   overflow-hidden"
        data-testid="chat-canvas"
      >
        <Outlet />
      </main>
    </div>
  );
};
✔ Glass sidebar
✔ Sticky cockpit feel
✔ Infinite canvas ready
✔ Named export (rule-compliant)

2️⃣ Create the Chat Canvas Page
📁 Create
swift
Copy code
/app/frontend/src/pages/ChatCanvas.jsx
🧩 ChatCanvas.jsx
jsx
Copy code
export default function ChatCanvas() {
  return (
    <div
      className="relative flex-1 overflow-y-auto px-4 md:px-8 pt-12 pb-40"
      data-testid="chat-scroll-area"
    >
      {/* Empty State */}
      <div
        className="flex flex-col items-center justify-center
                   min-h-[60vh] text-center
                   text-zinc-500"
        data-testid="empty-chat-state"
      >
        <p className="text-sm uppercase tracking-widest font-mono mb-4">
          No Active Messages
        </p>

        <h2 className="text-3xl md:text-4xl font-semibold tracking-tight text-zinc-200">
          The canvas is idle
        </h2>

        <p className="mt-4 max-w-md text-zinc-400">
          Start a transmission to bring the cockpit online.
        </p>
      </div>
    </div>
  );
}
✔ Infinite scroll area
✔ Bottom padding reserved for floating input
✔ Empty state ready for texture overlay (Phase 3)

3️⃣ Floating Input Capsule (Layout-Only)
This is NOT sticky.
It floats above the bottom edge, as required.

📁 Create
swift
Copy code
/app/frontend/src/components/FloatingInput.jsx
🧩 FloatingInput.jsx
jsx
Copy code
export const FloatingInput = () => {
  return (
    <div
      className="pointer-events-none absolute inset-x-0 bottom-6
                 flex justify-center z-40"
      data-testid="floating-input-wrapper"
    >
      <div
        className="pointer-events-auto w-full max-w-3xl mx-4
                   bg-zinc-800/50 backdrop-blur-xl
                   border border-white/10
                   rounded-2xl shadow-2xl"
        data-testid="floating-input-container"
      >
        <input
          type="text"
          placeholder="Transmit a message…"
          className="w-full bg-transparent
                     text-zinc-100 placeholder:text-zinc-500
                     px-6 py-6 rounded-2xl
                     focus:outline-none focus:ring-1
                     focus:ring-violet-500/40"
          data-testid="chat-input"
        />
      </div>
    </div>
  );
};
✔ Floating capsule
✔ Generous padding
✔ Glass effect
✔ No sticky positioning
✔ Pointer-events handled correctly

4️⃣ Wire Everything Together
🔧 Update App.js
jsx
Copy code
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { CockpitLayout } from "@/layouts/CockpitLayout";
import ChatCanvas from "@/pages/ChatCanvas";
import { FloatingInput } from "@/components/FloatingInput";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<CockpitLayout />}>
          <Route
            path="/"
            element={
              <>
                <ChatCanvas />
                <FloatingInput />
              </>
            }
          />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
