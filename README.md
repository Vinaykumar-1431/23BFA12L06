import React, { useState, useEffect } from "react";
import "./App.css";

function App() {
  const [originalUrl, setOriginalUrl] = useState("");
  const [urlList, setUrlList] = useState(() => {
    const saved = localStorage.getItem("urlList");
    return saved ? JSON.parse(saved) : [];
  });

  useEffect(() => {
    localStorage.setItem("urlList", JSON.stringify(urlList));
  }, [urlList]);

  const generateShortUrl = () => {
    const id = Math.random().toString(36).substring(2, 7);
    return `short.ly/${id}`;
  };

  const handleShorten = () => {
    if (!originalUrl.trim()) return;
    const shortUrl = generateShortUrl();
    const newUrl = {
      id: Date.now(),
      originalUrl,
      shortUrl,
      clicks: 0,
    };
    setUrlList([newUrl, ...urlList]);
    setOriginalUrl("");
  };

  const handleClick = (id) => {
    const updated = urlList.map((url) =>
      url.id === id ? { ...url, clicks: url.clicks + 1 } : url
    );
    setUrlList(updated);
  };

  return (
    <div className="App">
      <h1>🔗 URL Shortener</h1>
      <div className="input-container">
        <input
          type="text"
          value={originalUrl}
          onChange={(e) => setOriginalUrl(e.target.value)}
          placeholder="Enter long URL..."
        />
        <button onClick={handleShorten}>Shorten</button>
      </div>

      <h2>Analytics 📊</h2>
      <table>
        <thead>
          <tr>
            <th>Original URL</th>
            <th>Short URL</th>
            <th>Clicks</th>
          </tr>
        </thead>
        <tbody>
          {urlList.map((url) => (
            <tr key={url.id}>
              <td>{url.originalUrl}</td>
              <td>
                <a
                  href="#"
                  onClick={() => handleClick(url.id)}
                  style={{ color: "#007bff" }}
                >
                  {url.shortUrl}
                </a>
              </td>
              <td>{url.clicks}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default App;
