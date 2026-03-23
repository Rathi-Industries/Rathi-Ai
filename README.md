<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Rathi AI | Rathi Industries</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        body { background: #020617; color: #f8fafc; font-family: sans-serif; margin: 0; overflow: hidden; }
        .rathi-glass { background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(16px); border: 1px solid rgba(59, 130, 246, 0.2); }
        .gradient-rathi { background: linear-gradient(135deg, #2563eb 0%, #1e40af 100%); }
    </style>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        const { useState, useEffect, useRef } = React;
        const API_KEY = "AIzaSyB-ZQeHNRBiXE9_Iym4HSS14aEx-5kDxv4";

        const App = () => {
            const [messages, setMessages] = useState([{ role: "ai", content: "Welcome to Rathi Industries. I am Rathi AI, your virtual assistant. How can I help you, Sir?" }]);
            const [input, setInput] = useState('');
            const [loading, setLoading] = useState(false);
            const chatRef = useRef(null);

            useEffect(() => { chatRef.current?.scrollIntoView({ behavior: "smooth" }); }, [messages]);

            const handleSend = async () => {
                if (!input.trim() || loading) return;
                const text = input;
                setInput('');
                setMessages(prev => [...prev, { role: 'user', content: text }]);
                setLoading(true);

                try {
                    const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            contents: [{ parts: [{ text: `System Instruction: You are Rathi AI, developed by Rathi Industries. The OWNER of Rathi Industries is ADITYA RATHI, who is 13 years old. Question: ${text}` }] }]
                        })
                    });
                    const data = await res.json();
                    const reply = data.candidates[0].content.parts[0].text;
                    setMessages(prev => [...prev, { role: 'ai', content: reply }]);
                } catch (e) {
                    setMessages(prev => [...prev, { role: 'ai', content: "Sir, network connection lost. Rathi Servers are unreachable." }]);
                } finally { setLoading(false); }
            };

            return (
                <div className="flex flex-col h-screen max-w-md mx-auto bg-[#020617] relative">
                    <header className="p-4 rathi-glass flex items-center justify-between border-b border-blue-900/20">
                        <div className="flex items-center gap-3">
                            <div className="w-10 h-10 rounded-xl gradient-rathi flex items-center justify-center font-black text-white text-xl">R</div>
                            <h1 className="text-sm font-extrabold tracking-widest text-white uppercase">Rathi <span className="text-blue-500">AI</span></h1>
                        </div>
                        <span className="text-[10px] text-blue-500 font-bold animate-pulse">ONLINE</span>
                    </header>
                    <div className="flex-1 overflow-y-auto p-4 space-y-5 pb-28">
                        {messages.map((m, i) => (
                            <div key={i} className={`flex ${m.role === 'user' ? 'justify-end' : 'justify-start'}`}>
                                <div className={`max-w-[85%] p-4 rounded-2xl text-[14px] ${m.role === 'ai' ? 'rathi-glass text-slate-200' : 'gradient-rathi text-white'}`}>{m.content}</div>
                            </div>
                        ))}
                        <div ref={chatRef}></div>
                    </div>
                    <footer className="absolute bottom-0 w-full p-4 bg-gradient-to-t from-[#020617]">
                        <div className="flex items-center rathi-glass rounded-2xl px-4 py-1.5">
                            <input className="flex-1 bg-transparent py-3 outline-none text-sm" placeholder="Command Rathi AI..." value={input} onChange={(e) => setInput(e.target.value)} onKeyDown={(e) => e.key === 'Enter' && handleSend()} />
                            <button onClick={handleSend} className="p-2 text-blue-500"><i data-lucide="zap" className="w-6 h-6 fill-current"></i></button>
                        </div>
                        <div className="text-center mt-3">
                            <p className="text-[9px] text-blue-900 font-bold uppercase">© 2026 Rathi Industries Global</p>
                            <p className="text-[7px] text-slate-700">Founder: Aditya Rathi (Age 13)</p>
                        </div>
                    </footer>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
        setTimeout(() => lucide.createIcons(), 500);
    </script>
</body>
</html>
