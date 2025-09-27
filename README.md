git clone https://github.com/Hazrat90/farcaster-mini-app.git
cd farcaster-mini-app
npx create-react-app farcaster-mini-app
cd farcaster-mini-app
npm install axios ethers tailwindcss
npx tailwindcss init
farcaster-mini-app/
│
├─ public/               # Images, icons
├─ src/
│   ├─ components/       # Reusable UI components
│   │   ├─ Poll.jsx
│   │   ├─ Leaderboard.jsx
│   │   ├─ NFTMint.jsx
│   │   ├─ Game.jsx
│   │   ├─ Tracker.jsx
│   │   └─ DataDisplay.jsx
│   ├─ App.jsx
│   ├─ services/         # API or blockchain logic
│   │   ├─ api.js
│   │   └─ blockchain.js
│   └─ styles/           # TailwindCSS or custom styles
├─ package.json
└─ README.md
import React, { useState } from "react";

const Poll = ({ question, options }) => {
  const [selected, setSelected] = useState(null);
  const [votes, setVotes] = useState(options.map(() => 0));

  const vote = (index) => {
    const newVotes = [...votes];
    newVotes[index]++;
    setVotes(newVotes);
    setSelected(index);
  };

  return (
    <div className="p-4 border rounded shadow">
      <h2 className="text-xl font-bold">{question}</h2>
      {options.map((opt, idx) => (
        <button
          key={idx}
          onClick={() => vote(idx)}
          className="block mt-2 p-2 bg-blue-500 text-white rounded"
        >
          {opt} - {votes[idx]} votes
        </button>
      ))}
      {selected !== null && <p>You voted for: {options[selected]}</p>}
    </div>
  );
};

export default Poll;
import React from "react";

const Leaderboard = ({ players }) => {
  return (
    <div className="p-4 border rounded shadow">
      <h2 className="text-xl font-bold mb-2">Leaderboard</h2>
      <ul>
        {players.sort((a,b) => b.score - a.score).map((player, idx) => (
          <li key={idx} className="mb-1">{idx + 1}. {player.name} - {player.score}</li>
        ))}
      </ul>
    </div>
  );
};

export default Leaderboard;
import React, { useState } from "react";
import { ethers } from "ethers";
import { mintNFT } from "../services/blockchain";

const NFTMint = () => {
  const [status, setStatus] = useState("");

  const handleMint = async () => {
    setStatus("Minting...");
    try {
      await mintNFT(); // function defined in blockchain.js
      setStatus("NFT minted successfully!");
    } catch (err) {
      setStatus("Error minting NFT");
    }
  };

  return (
    <div className="p-4 border rounded shadow">
      <button onClick={handleMint} className="p-2 bg-green-500 text-white rounded">
        Mint NFT
      </button>
      <p>{status}</p>
    </div>
  );
};

export default NFTMint;
import { ethers } from "ethers";

export const mintNFT = async () => {
  if (!window.ethereum) throw new Error("Wallet not detected");
  await window.ethereum.request({ method: "eth_requestAccounts" });
  const provider = new ethers.BrowserProvider(window.ethereum);
  const signer = await provider.getSigner();
  const contractAddress = "YOUR_CONTRACT_ADDRESS";
  const abi = [ /* YOUR NFT CONTRACT ABI */ ];
  const contract = new ethers.Contract(contractAddress, abi, signer);
  const tx = await contract.mintNFT();
  await tx.wait();
};
git add .
git commit -m "Initial mini app"
git push origin main
