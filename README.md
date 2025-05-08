# handballteam
import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

const players = [
  "山田", "佐藤", "鈴木", "田中", "高橋", "伊藤", "渡辺"
];

export default function HandballScoreApp() {
  const [score, setScore] = useState({ home: 0, away: 0 });
  const [events, setEvents] = useState([]);
  const [team, setTeam] = useState("home");
  const [selectedPlayer, setSelectedPlayer] = useState("");

  const addEvent = (type) => {
    if (!selectedPlayer) return;
    const newEvent = {
      type,
      team,
      player: selectedPlayer,
      time: new Date().toLocaleTimeString(),
    };
    setEvents([...events, newEvent]);
    if (type === "goal") {
      setScore({ ...score, [team]: score[team] + 1 });
    }
  };

  return (
    <div className="p-6 space-y-6">
      <h1 className="text-2xl font-bold">ハンドボール試合管理</h1>
      <div className="grid grid-cols-2 gap-4">
        <Card>
          <CardContent className="text-center py-4">
            <h2 className="text-xl font-semibold">ホーム</h2>
            <p className="text-4xl">{score.home}</p>
          </CardContent>
        </Card>
        <Card>
          <CardContent className="text-center py-4">
            <h2 className="text-xl font-semibold">アウェイ</h2>
            <p className="text-4xl">{score.away}</p>
          </CardContent>
        </Card>
      </div>
      <div className="space-y-4">
        <div>
          <label className="block mb-2">チーム選択:</label>
          <select onChange={(e) => setTeam(e.target.value)} value={team} className="border p-2 rounded">
            <option value="home">ホーム</option>
            <option value="away">アウェイ</option>
          </select>
        </div>
        <div>
          <label className="block mb-2">選手選択:</label>
          <select onChange={(e) => setSelectedPlayer(e.target.value)} value={selectedPlayer} className="border p-2 rounded">
            <option value="">-- 選択 --</option>
            {players.map((p) => (
              <option key={p} value={p}>{p}</option>
            ))}
          </select>
        </div>
        <div className="flex gap-2 flex-wrap">
          <Button onClick={() => addEvent("goal")}>得点</Button>
          <Button onClick={() => addEvent("warning")}>警告</Button>
          <Button onClick={() => addEvent("suspension")}>2分間退場</Button>
          <Button onClick={() => addEvent("save")}>キーパーセーブ</Button>
        </div>
      </div>
      <div className="mt-6">
        <h2 className="text-lg font-semibold mb-2">試合イベント履歴</h2>
        <ul className="space-y-1">
          {events.map((e, i) => (
            <li key={i}>
              [{e.time}] {e.team === "home" ? "ホーム" : "アウェイ"} - {e.player} が
              {e.type === "goal" && "得点！"}
              {e.type === "warning" && "警告を受けた！"}
              {e.type === "suspension" && "2分間退場！"}
              {e.type === "save" && "セーブ！"}
            </li>
          ))}
        </ul>
      </div>
    </div>
