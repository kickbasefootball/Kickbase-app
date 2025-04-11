import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Select, SelectItem } from "@/components/ui/select";
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";
import { Sparkles, Users, Trophy, Target, MapPin, ClipboardList } from "lucide-react";

export default function KickBaseApp() {
  const [teamName, setTeamName] = useState("");
  const [players, setPlayers] = useState([]);
  const [newPlayer, setNewPlayer] = useState("");
  const [matchLog, setMatchLog] = useState("");
  const [location, setLocation] = useState("");
  const [skill, setSkill] = useState("");
  const [availablePlayers, setAvailablePlayers] = useState([]);
  const [ratings, setRatings] = useState({});
  const [leaderboard, setLeaderboard] = useState([
    { name: "Leo", goals: 12 },
    { name: "Max", goals: 10 },
    { name: "Ali", goals: 8 }
  ]);
  const [drills, setDrills] = useState("");

  const addPlayer = () => {
    if (newPlayer.trim() !== "") {
      setPlayers([...players, newPlayer.trim()]);
      setNewPlayer("");
    }
  };

  const searchPlayers = () => {
    setAvailablePlayers(["Tom", "Ali", "Jana"]);
  };

  const ratePlayer = (name, score) => {
    setRatings({ ...ratings, [name]: score });
  };

  return (
    <div className="p-6 max-w-3xl mx-auto space-y-6 bg-gradient-to-br from-green-50 to-white shadow-xl rounded-2xl">
      <h1 className="text-3xl font-extrabold text-center text-green-700 flex items-center justify-center gap-2">
        <Sparkles className="w-6 h-6" /> KickBase Amateur
      </h1>

      <Tabs defaultValue="team">
        <TabsList className="grid grid-cols-6 gap-1 bg-green-100 p-1 rounded-xl shadow-sm">
          <TabsTrigger value="team" className="flex items-center gap-1"><Users className="w-4 h-4" />Team</TabsTrigger>
          <TabsTrigger value="match" className="flex items-center gap-1"><ClipboardList className="w-4 h-4" />Spiel</TabsTrigger>
          <TabsTrigger value="platz" className="flex items-center gap-1"><MapPin className="w-4 h-4" />Platz</TabsTrigger>
          <TabsTrigger value="trainer" className="flex items-center gap-1"><Target className="w-4 h-4" />Trainer</TabsTrigger>
          <TabsTrigger value="rating" className="flex items-center gap-1"><Trophy className="w-4 h-4" />Bewertung</TabsTrigger>
          <TabsTrigger value="community" className="flex items-center gap-1"><Sparkles className="w-4 h-4" />Community</TabsTrigger>
        </TabsList>

        <TabsContent value="team">
          <Card className="rounded-xl border border-green-200">
            <CardContent className="space-y-4 mt-4">
              <Input
                className="rounded-xl"
                placeholder="Teamname (z.B. FC Bolzplatz)"
                value={teamName}
                onChange={(e) => setTeamName(e.target.value)}
              />
              <div className="flex gap-2">
                <Input
                  className="rounded-xl"
                  placeholder="Spielername"
                  value={newPlayer}
                  onChange={(e) => setNewPlayer(e.target.value)}
                />
                <Button className="rounded-xl" onClick={addPlayer}>Hinzufügen</Button>
              </div>
              <ul className="list-disc pl-5 text-sm">
                {players.map((player, index) => (
                  <li key={index}>{player}</li>
                ))}
              </ul>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="match">
          <Card className="rounded-xl border border-green-200">
            <CardContent className="space-y-4 mt-4">
              <Textarea
                className="rounded-xl"
                placeholder="z.B. 3:2 Sieg gegen Team X, Torschützen: Max (2), Leo (1)"
                value={matchLog}
                onChange={(e) => setMatchLog(e.target.value)}
              />
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="platz">
          <Card className="rounded-xl border border-green-200">
            <CardContent className="space-y-4 mt-4">
              <Input
                className="rounded-xl"
                placeholder="Standort (z.B. Berlin, Bolzplatzstraße 1)"
                value={location}
                onChange={(e) => setLocation(e.target.value)}
              />
              <Select onValueChange={(val) => setSkill(val)}>
                <SelectItem value="Anfänger">Anfänger</SelectItem>
                <SelectItem value="Fortgeschritten">Fortgeschritten</SelectItem>
                <SelectItem value="Profi">Profi</SelectItem>
              </Select>
              <Button className="rounded-xl" onClick={searchPlayers}>Spieler suchen</Button>
              <ul className="list-disc pl-5 text-sm">
                {availablePlayers.map((player, idx) => (
                  <li key={idx}>{player}</li>
                ))}
              </ul>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="trainer">
          <Card className="rounded-xl border border-green-200">
            <CardContent className="space-y-4 mt-4">
              <Textarea
                className="rounded-xl"
                placeholder="Trainingsnotizen & Übungen"
                value={drills}
                onChange={(e) => setDrills(e.target.value)}
              />
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="rating">
          <Card className="rounded-xl border border-green-200">
            <CardContent className="space-y-4 mt-4">
              {players.map((player, idx) => (
                <div key={idx} className="flex items-center gap-2">
                  <span className="w-32 font-medium text-sm">{player}</span>
                  <Input
                    className="rounded-xl"
                    type="number"
                    placeholder="0–10"
                    min="0"
                    max="10"
                    onChange={(e) => ratePlayer(player, e.target.value)}
                  />
                </div>
              ))}
              <pre className="text-xs text-gray-500 bg-gray-50 p-2 rounded-xl">{JSON.stringify(ratings, null, 2)}</pre>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="community">
          <Card className="rounded-xl border border-green-200">
            <CardContent className="space-y-4 mt-4">
              <h2 className="text-lg font-semibold">Top 3 Torschützen</h2>
              <ul className="list-decimal pl-5 text-sm">
                {leaderboard.map((entry, idx) => (
                  <li key={idx}>{entry.name} – {entry.goals} Tore</li>
                ))}
              </ul>
              <p className="text-sm text-gray-600">Weitere Community-Features folgen bald!</p>
            </CardContent>
          </Card>
        </TabsContent>
      </Tabs>
    </div>
  );
}

