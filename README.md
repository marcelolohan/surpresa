import React, { useState, useEffect } from "react";
import Confetti from "react-confetti";

export default function BabyReveal() {
  const [showSuspense, setShowSuspense] = useState(true);
  const [selectedGender, setSelectedGender] = useState(null);
  const [showMessageInput, setShowMessageInput] = useState(false);
  const [message, setMessage] = useState("");
  const [senderName, setSenderName] = useState("");
  const [messages, setMessages] = useState([]);
  const [currentTextIndex, setCurrentTextIndex] = useState(0);
  const [currentText, setCurrentText] = useState("");
  const [showButton, setShowButton] = useState(false);

  const suspenseTexts = [
    { text: "Está chegando uma surpresa", delay: 5000 },
    { text: "Já consegue imaginar? rs", delay: 5000 },
    { text: "No dia 15/04/205 descobrimos que a nossa vida iria mudar", delay: 5000 },
    { text: "E você é especial para nós e gostariamos de compartilhar essa grande Noticia", delay: 7000 },
    { text: "É isso mesmo, Temos um Bebê a caminho", delay: 5000 },
    { text: "Deixe seu palpite e sua mensagem", delay: 5000 }
  ];

  useEffect(() => {
    if (currentTextIndex < suspenseTexts.length) {
      setCurrentText(suspenseTexts[currentTextIndex].text);
      const timer = setTimeout(() => {
        setCurrentTextIndex((prev) => prev + 1);
      }, suspenseTexts[currentTextIndex].delay);
      return () => clearTimeout(timer);
    } else {
      setShowSuspense(false);
    }
  }, [currentTextIndex]);

  const handleGenderSelection = (gender) => {
    setSelectedGender(gender);
    setShowMessageInput(true);
  };

  const handleSendMessage = () => {
    if (message.trim() !== "" && senderName.trim() !== "") {
      setMessages((prevMessages) => [...prevMessages, { name: senderName, text: message }]);
      setMessage("");
      setSenderName("");
    }
  };

  return (
    <div className="min-h-screen flex flex-col items-center justify-center bg-pink-50 text-center p-6 space-y-6">
      {showSuspense ? (
        <>
          <h1 className="text-4xl font-bold text-pink-600 animate-pulse">
            {currentText}
          </h1>
        </>
      ) : !selectedGender ? (
        <>
          <div className="flex gap-6 mt-6">
            <button
              onClick={() => handleGenderSelection("menino")}
              className="bg-blue-500 text-white px-6 py-3 rounded-xl shadow-lg hover:bg-blue-600 transition"
            >
              Menino
            </button>
            <button
              onClick={() => handleGenderSelection("menina")}
              className="bg-pink-500 text-white px-6 py-3 rounded-xl shadow-lg hover:bg-pink-600 transition"
            >
              Menina
            </button>
          </div>
        </>
      ) : (
        <div className="space-y-4">
          <Confetti />
          <h2 className="text-3xl font-semibold text-gray-800">
            Ainda não sabemos o sexo do bebê, mas estamos muito felizes!
          </h2>

          {showMessageInput && (
            <div className="w-full max-w-md mx-auto space-y-3">
              <input
                type="text"
                placeholder="Seu nome"
                className="w-full p-3 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400"
                value={senderName}
                onChange={(e) => setSenderName(e.target.value)}
              />
              <textarea
                className="w-full p-3 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-pink-400"
                placeholder="Deixe sua mensagem para a gente..."
                value={message}
                onChange={(e) => setMessage(e.target.value)}
              />
              <button
                onClick={handleSendMessage}
                className="bg-green-500 text-white px-4 py-2 rounded-xl hover:bg-green-600 transition w-full"
              >
                Enviar mensagem
              </button>
            </div>
          )}

          <div className="w-full max-w-md mt-6 space-y-2">
            {messages.map((msg, index) => (
              <div
                key={index}
                className="bg-white p-3 rounded-xl shadow-md text-left"
              >
                <strong>{msg.name}</strong>: {msg.text}
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}
