Health-game-for-students (react)
· typescript
/*
      <p className="text-sm text-gray-600">Tap to mark today's habits. Small daily wins build long-term health.</p>
      <div className="mt-3 grid gap-2">
        {HABITS.map((h, i) => (
          <button key={i} onClick={() => toggleHabit(i)} className={`p-2 text-left rounded-md border ${today[i] ? "bg-green-50 border-green-400" : "border-gray-200"}`}>
            <div className="flex justify-between">
              <div>{h}</div>
              <div className="text-sm text-gray-500">Streak: {getStreak(i)}</div>
            </div>
          </button>
        ))}
      </div>
    </div>
  );
}


// ---------- Main Game Wrapper ----------
export default function HealthGameLauncher() {
  const [score, setScore] = useState(0);
  const [activeTab, setActiveTab] = useState("quiz");


  function handleScore(delta) {
    setScore((s) => Math.max(0, s + delta));
  }


  return (
    <div className="min-h-screen p-6 bg-gradient-to-b from-sky-50 to-white">
      <div className="max-w-3xl mx-auto">
        <header className="flex items-center justify-between mb-4">
          <h1 className="text-2xl font-bold">Healthy Habits — Mini Games</h1>
          <div className="text-right">
            <div className="text-sm text-gray-500">Points</div>
            <div className="text-xl font-semibold">{score}</div>
          </div>
        </header>


        <nav className="flex gap-2 mb-4">
          <button onClick={() => setActiveTab("quiz")} className={`px-3 py-1 rounded ${activeTab === "quiz" ? "bg-indigo-600 text-white" : "border"}`}>Quiz</button>
          <button onClick={() => setActiveTab("catch")} className={`px-3 py-1 rounded ${activeTab === "catch" ? "bg-indigo-600 text-white" : "border"}`}>Catch</button>
          <button onClick={() => setActiveTab("habit")} className={`px-3 py-1 rounded ${activeTab === "habit" ? "bg-indigo-600 text-white" : "border"}`}>Habit</button>
        </nav>


        <main>
          {activeTab === "quiz" && <QuickQuiz onScore={handleScore} />}
          {activeTab === "catch" && <CatchTheSnack onScore={handleScore} />}
          {activeTab === "habit" && <HabitTracker />}


          <section className="mt-6 p-4 rounded-lg border bg-white">
            <h4 className="font-semibold">Design notes for teachers / designers</h4>
            <ul className="mt-2 list-disc ml-5 text-sm text-gray-700">
              <li>Session length &lt; 5 minutes keeps attention — use these mini-modules between classes.</li>
              <li>Give immediate feedback (quiz tips, score changes) to reinforce learning.</li>
              <li>Use small rewards and public recognition (class leaderboard) to motivate — keep it positive and voluntary.</li>
              <li>Pair gameplay with discussion: after a round, spend 2 minutes to reflect on what changed and make an action plan.</li>
              <li>Use habit tracker for long-term behaviour: encourage students to log 1–2 achievable habits, then gradually add more.</li>
              <li>Ensure privacy and safety: do not collect identifying data unless you have permission; prefer local storage or anonymous leaderboards.</li>
            </ul>
          </section>
        </main>


        <footer className="mt-6 text-xs text-gray-500 text-center">
          Built for schools — adapt questions and visuals for age group. Keyboard accessible (arrow keys) and mobile friendly.
        </footer>
      </div>
    </div>
  );
}
