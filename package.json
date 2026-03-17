import { useState, useEffect } from "react";

const DEMO_USER = { user: "miguel", password: "gatos123" };
const STORAGE_KEY = "misgatos_cats";

const INITIAL_CATS = [
  { id: 1, name: "Luna", breed: "Siamés", age: 3, color: "#b8a9c9", emoji: "🐱", weight: "4.2 kg", chip: "ES123456789", notes: "Le encanta dormir al sol", vaccinated: true },
  { id: 2, name: "Mochi", breed: "Maine Coon", age: 5, color: "#e8c99a", emoji: "😸", weight: "6.8 kg", chip: "ES987654321", notes: "Muy cariñoso por las mañanas", vaccinated: true },
];

const pawPrint = `<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" fill="currentColor" opacity="0.07"><ellipse cx="50" cy="65" rx="22" ry="18"/><ellipse cx="25" cy="42" rx="10" ry="13"/><ellipse cx="75" cy="42" rx="10" ry="13"/><ellipse cx="38" cy="28" rx="8" ry="11"/><ellipse cx="62" cy="28" rx="8" ry="11"/></svg>`;
const encodedPaw = `data:image/svg+xml,${encodeURIComponent(pawPrint)}`;

export default function App() {
  const [screen, setScreen] = useState("login");
  const [loginData, setLoginData] = useState({ user: "", password: "" });
  const [loginError, setLoginError] = useState("");
  const [cats, setCats] = useState(() => {
    try {
      const saved = localStorage.getItem(STORAGE_KEY);
      return saved ? JSON.parse(saved) : INITIAL_CATS;
    } catch { return INITIAL_CATS; }
  });
  const [selectedCat, setSelectedCat] = useState(null);
  const [showForm, setShowForm] = useState(false);
  const [editingCat, setEditingCat] = useState(null);
  const [formData, setFormData] = useState({ name: "", breed: "", age: "", color: "#ff9eb5", emoji: "🐱", weight: "", chip: "", notes: "", vaccinated: false });
  const [shake, setShake] = useState(false);
  const [deleteConfirm, setDeleteConfirm] = useState(null);
  const [showInstallTip, setShowInstallTip] = useState(false);

  const EMOJIS = ["🐱","😸","😹","😺","😻","🐈","🐈‍⬛","😾","😼","🙀"];
  const COLORS = ["#ff9eb5","#ffcba4","#b8e0d2","#b8a9c9","#a8d8ea","#f9e4b7","#c9e4ca","#f4b8c1","#d4a5c9","#a5c8d4"];

  // Persist cats to localStorage
  useEffect(() => {
    try { localStorage.setItem(STORAGE_KEY, JSON.stringify(cats)); } catch {}
  }, [cats]);

  // Register service worker
  useEffect(() => {
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('/sw.js').catch(() => {});
    }
    // Show install tip on iOS Safari after login
    const isIOS = /iphone|ipad|ipod/i.test(navigator.userAgent);
    const isStandalone = window.navigator.standalone;
    const seen = localStorage.getItem('install_tip_seen');
    if (isIOS && !isStandalone && !seen) setShowInstallTip(true);
  }, []);

  const handleLogin = () => {
    if (loginData.user === DEMO_USER.user && loginData.password === DEMO_USER.password) {
      setScreen("home");
      setLoginError("");
    } else {
      setLoginError("Usuario o contraseña incorrectos 😿");
      setShake(true);
      setTimeout(() => setShake(false), 500);
    }
  };

  const openAdd = () => {
    setEditingCat(null);
    setFormData({ name: "", breed: "", age: "", color: "#ff9eb5", emoji: "🐱", weight: "", chip: "", notes: "", vaccinated: false });
    setShowForm(true);
  };

  const openEdit = (cat) => {
    setEditingCat(cat);
    setFormData({ ...cat });
    setShowForm(true);
    setSelectedCat(null);
  };

  const saveForm = () => {
    if (!formData.name.trim()) return;
    if (editingCat) {
      setCats(prev => prev.map(c => c.id === editingCat.id ? { ...formData, id: editingCat.id } : c));
    } else {
      setCats(prev => [...prev, { ...formData, id: Date.now(), age: Number(formData.age) }]);
    }
    setShowForm(false);
  };

  const deleteCat = (id) => {
    setCats(prev => prev.filter(c => c.id !== id));
    setDeleteConfirm(null);
    setSelectedCat(null);
  };

  const s = {
    app: { minHeight: "100dvh", fontFamily: "'Nunito', sans-serif", background: "#fdf6ee", position: "relative", overflow: "hidden" },
    bgPattern: { position: "fixed", inset: 0, backgroundImage: `url("${encodedPaw}")`, backgroundSize: "120px", zIndex: 0, pointerEvents: "none" },
    loginWrap: { minHeight: "100dvh", display: "flex", alignItems: "center", justifyContent: "center", position: "relative", zIndex: 1, padding: "20px" },
    loginCard: { background: "white", borderRadius: "32px", padding: "48px 36px", width: "100%", maxWidth: "360px", boxShadow: "0 20px 60px rgba(0,0,0,0.12)", textAlign: "center" },
    loginLogo: { fontSize: "72px", lineHeight: 1, marginBottom: "8px" },
    loginTitle: { fontSize: "28px", fontWeight: 900, color: "#2d2d2d", margin: "0 0 4px" },
    loginSub: { color: "#999", fontSize: "14px", marginBottom: "32px" },
    input: { width: "100%", padding: "14px 18px", borderRadius: "16px", border: "2px solid #f0e6e0", fontSize: "16px", fontFamily: "inherit", outline: "none", boxSizing: "border-box", marginBottom: "12px", background: "#fdf6ee" },
    loginBtn: { width: "100%", padding: "16px", borderRadius: "20px", border: "none", background: "linear-gradient(135deg, #ff9eb5, #ffb347)", color: "white", fontFamily: "inherit", fontSize: "16px", fontWeight: 800, cursor: "pointer", marginTop: "8px", boxShadow: "0 6px 20px rgba(255,158,181,0.5)" },
    loginHint: { marginTop: "20px", padding: "12px", borderRadius: "12px", background: "#fff8f0", fontSize: "12px", color: "#aaa" },
    errorMsg: { color: "#e06060", fontSize: "13px", marginTop: "6px", fontWeight: 700 },

    homeWrap: { position: "relative", zIndex: 1, maxWidth: "480px", margin: "0 auto", padding: "0 16px 100px" },
    header: { display: "flex", alignItems: "center", justifyContent: "space-between", padding: "env(safe-area-inset-top, 24px) 0 16px", paddingTop: "max(env(safe-area-inset-top), 24px)" },
    headerTitle: { fontSize: "26px", fontWeight: 900, color: "#2d2d2d" },
    headerSub: { fontSize: "13px", color: "#aaa", fontWeight: 600 },
    avatarBtn: { width: "44px", height: "44px", borderRadius: "50%", background: "linear-gradient(135deg, #ff9eb5, #ffb347)", border: "none", fontSize: "20px", cursor: "pointer" },

    catGrid: { display: "grid", gridTemplateColumns: "1fr 1fr", gap: "14px", marginTop: "8px" },
    catCard: { background: "white", borderRadius: "24px", padding: "20px 16px", boxShadow: "0 4px 20px rgba(0,0,0,0.07)", cursor: "pointer", textAlign: "center", border: "2px solid transparent" },
    catEmoji: { fontSize: "44px", lineHeight: 1, marginBottom: "8px" },
    catName: { fontWeight: 900, fontSize: "16px", color: "#2d2d2d", marginBottom: "2px" },
    catBreed: { fontSize: "12px", color: "#bbb", fontWeight: 600 },
    catAge: { fontSize: "12px", color: "#ddd", marginTop: "6px" },
    catDot: { width: "10px", height: "10px", borderRadius: "50%", margin: "8px auto 0", display: "block" },
    badge: { display: "inline-flex", alignItems: "center", gap: "4px", padding: "4px 10px", borderRadius: "20px", fontSize: "11px", fontWeight: 800 },

    addBtn: { position: "fixed", bottom: "max(28px, env(safe-area-inset-bottom, 28px))", left: "50%", transform: "translateX(-50%)", background: "linear-gradient(135deg, #ff9eb5, #ffb347)", border: "none", borderRadius: "28px", padding: "16px 36px", color: "white", fontFamily: "inherit", fontWeight: 900, fontSize: "15px", cursor: "pointer", boxShadow: "0 8px 28px rgba(255,158,181,0.55)", zIndex: 10 },

    overlay: { position: "fixed", inset: 0, background: "rgba(0,0,0,0.35)", zIndex: 20, display: "flex", alignItems: "flex-end", justifyContent: "center" },
    sheet: { background: "white", borderRadius: "32px 32px 0 0", width: "100%", maxWidth: "480px", padding: "28px 24px", paddingBottom: "max(40px, env(safe-area-inset-bottom, 40px))", maxHeight: "90dvh", overflowY: "auto" },
    sheetHandle: { width: "40px", height: "4px", borderRadius: "4px", background: "#eee", margin: "0 auto 20px" },

    detailEmoji: { fontSize: "64px", textAlign: "center", display: "block", marginBottom: "8px" },
    detailName: { textAlign: "center", fontSize: "26px", fontWeight: 900, color: "#2d2d2d" },
    detailBreed: { textAlign: "center", color: "#bbb", fontWeight: 600, marginBottom: "20px" },
    detailGrid: { display: "grid", gridTemplateColumns: "1fr 1fr", gap: "10px", marginBottom: "16px" },
    detailItem: { background: "#fdf6ee", borderRadius: "16px", padding: "12px 14px" },
    detailLabel: { fontSize: "11px", color: "#ccc", fontWeight: 700, textTransform: "uppercase", letterSpacing: "0.5px" },
    detailVal: { fontSize: "15px", fontWeight: 800, color: "#2d2d2d", marginTop: "2px" },
    notesBox: { background: "#fdf6ee", borderRadius: "16px", padding: "14px", marginBottom: "20px" },
    actionRow: { display: "flex", gap: "10px" },
    editBtn: { flex: 1, padding: "14px", borderRadius: "18px", border: "2px solid #f0e6e0", background: "white", fontFamily: "inherit", fontWeight: 800, fontSize: "14px", cursor: "pointer", color: "#888" },
    deleteBtn: { flex: 1, padding: "14px", borderRadius: "18px", border: "none", background: "#fff0f0", fontFamily: "inherit", fontWeight: 800, fontSize: "14px", cursor: "pointer", color: "#e06060" },

    formTitle: { fontSize: "22px", fontWeight: 900, color: "#2d2d2d", marginBottom: "20px" },
    label: { fontSize: "12px", fontWeight: 800, color: "#bbb", textTransform: "uppercase", letterSpacing: "0.5px", marginBottom: "6px", display: "block" },
    emojiRow: { display: "flex", gap: "8px", flexWrap: "wrap", marginBottom: "16px" },
    emojiOpt: { fontSize: "28px", cursor: "pointer", padding: "6px", borderRadius: "12px", border: "2px solid transparent" },
    colorRow: { display: "flex", gap: "8px", flexWrap: "wrap", marginBottom: "16px" },
    colorOpt: { width: "28px", height: "28px", borderRadius: "50%", cursor: "pointer", border: "3px solid transparent" },
    vacRow: { display: "flex", alignItems: "center", gap: "10px", marginBottom: "20px" },
    toggle: { width: "44px", height: "24px", borderRadius: "12px", border: "none", cursor: "pointer", position: "relative", flexShrink: 0 },
    saveBtn: { width: "100%", padding: "16px", borderRadius: "20px", border: "none", background: "linear-gradient(135deg, #ff9eb5, #ffb347)", color: "white", fontFamily: "inherit", fontWeight: 900, fontSize: "16px", cursor: "pointer", boxShadow: "0 6px 20px rgba(255,158,181,0.4)" },

    installBanner: { position: "fixed", bottom: "90px", left: "16px", right: "16px", background: "white", borderRadius: "20px", padding: "16px", boxShadow: "0 8px 30px rgba(0,0,0,0.15)", zIndex: 15, display: "flex", alignItems: "center", gap: "12px" },
  };

  return (
    <div style={s.app}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&display=swap');
        @keyframes slideUp { from { transform: translateY(30px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        @keyframes shake { 0%,100%{transform:translateX(0)} 20%{transform:translateX(-8px)} 40%{transform:translateX(8px)} 60%{transform:translateX(-5px)} 80%{transform:translateX(5px)} }
        input:focus { border-color: #ff9eb5 !important; }
        .cat-card:active { transform: scale(0.97) !important; }
        * { -webkit-tap-highlight-color: transparent; }
      `}</style>
      <div style={s.bgPattern} />

      {/* LOGIN */}
      {screen === "login" && (
        <div style={s.loginWrap}>
          <div style={{ ...s.loginCard, animation: shake ? "shake 0.4s ease" : "slideUp 0.5s ease" }}>
            <div style={s.loginLogo}>🐾</div>
            <h1 style={s.loginTitle}>MisGatos</h1>
            <p style={s.loginSub}>La app de tu hogar felino 🏠</p>
            <input style={s.input} placeholder="Usuario" value={loginData.user} autoCapitalize="none" autoCorrect="off"
              onChange={e => setLoginData(p => ({ ...p, user: e.target.value }))}
              onKeyDown={e => e.key === "Enter" && handleLogin()} />
            <input style={s.input} type="password" placeholder="Contraseña" value={loginData.password}
              onChange={e => setLoginData(p => ({ ...p, password: e.target.value }))}
              onKeyDown={e => e.key === "Enter" && handleLogin()} />
            {loginError && <p style={s.errorMsg}>{loginError}</p>}
            <button style={s.loginBtn} onClick={handleLogin}>Entrar 🐾</button>
            <div style={s.loginHint}>
              Demo: <strong>miguel</strong> / <strong>gatos123</strong>
            </div>
          </div>
        </div>
      )}

      {/* HOME */}
      {screen === "home" && (
        <div style={s.homeWrap}>
          <div style={s.header}>
            <div>
              <div style={s.headerTitle}>Mis Gatos 🐾</div>
              <div style={s.headerSub}>{cats.length} peludo{cats.length !== 1 ? "s" : ""} en casa</div>
            </div>
            <button style={s.avatarBtn} onClick={() => { setScreen("login"); setLoginData({ user: "", password: "" }); }}>🐱</button>
          </div>

          {cats.length === 0 && (
            <div style={{ textAlign: "center", padding: "60px 20px", color: "#ccc" }}>
              <div style={{ fontSize: "64px", marginBottom: "12px" }}>😿</div>
              <div style={{ fontWeight: 800, fontSize: "18px" }}>No hay gatos aún</div>
              <div style={{ fontSize: "14px", marginTop: "4px" }}>¡Añade tu primer peludo!</div>
            </div>
          )}

          <div style={s.catGrid}>
            {cats.map(cat => (
              <div key={cat.id} className="cat-card" style={{ ...s.catCard, borderColor: cat.color + "55" }}
                onClick={() => setSelectedCat(cat)}>
                <div style={{ ...s.catDot, background: cat.color, marginBottom: "6px", marginTop: 0 }} />
                <div style={s.catEmoji}>{cat.emoji}</div>
                <div style={s.catName}>{cat.name}</div>
                <div style={s.catBreed}>{cat.breed}</div>
                <div style={s.catAge}>{cat.age} año{cat.age !== 1 ? "s" : ""}</div>
                {cat.vaccinated && (
                  <div style={{ ...s.badge, background: "#e8f8ee", color: "#5aab6e", marginTop: "8px" }}>✓ Vacunado</div>
                )}
              </div>
            ))}
          </div>

          <button style={s.addBtn} onClick={openAdd}>+ Añadir gato</button>

          {/* Install tip for iOS */}
          {showInstallTip && (
            <div style={s.installBanner}>
              <span style={{ fontSize: "28px" }}>📲</span>
              <div style={{ flex: 1 }}>
                <div style={{ fontWeight: 800, fontSize: "13px", color: "#2d2d2d" }}>Instalar en iPhone</div>
                <div style={{ fontSize: "12px", color: "#aaa", marginTop: "2px" }}>Pulsa <strong>Compartir ↑</strong> → "Añadir a inicio"</div>
              </div>
              <button style={{ background: "none", border: "none", fontSize: "18px", cursor: "pointer", color: "#ccc" }}
                onClick={() => { setShowInstallTip(false); localStorage.setItem('install_tip_seen', '1'); }}>✕</button>
            </div>
          )}
        </div>
      )}

      {/* DETALLE */}
      {selectedCat && (
        <div style={s.overlay} onClick={e => e.target === e.currentTarget && setSelectedCat(null)}>
          <div style={s.sheet}>
            <div style={s.sheetHandle} />
            <span style={s.detailEmoji}>{selectedCat.emoji}</span>
            <div style={s.detailName}>{selectedCat.name}</div>
            <div style={s.detailBreed}>{selectedCat.breed}</div>
            <div style={s.detailGrid}>
              <div style={s.detailItem}><div style={s.detailLabel}>Edad</div><div style={s.detailVal}>{selectedCat.age} año{selectedCat.age !== 1 ? "s" : ""}</div></div>
              <div style={s.detailItem}><div style={s.detailLabel}>Peso</div><div style={s.detailVal}>{selectedCat.weight || "—"}</div></div>
              <div style={{ ...s.detailItem, gridColumn: "1 / -1" }}><div style={s.detailLabel}>Microchip</div><div style={{ ...s.detailVal, fontSize: "13px", letterSpacing: "1px" }}>{selectedCat.chip || "—"}</div></div>
              <div style={s.detailItem}><div style={s.detailLabel}>Vacunado</div><div style={s.detailVal}>{selectedCat.vaccinated ? "✅ Sí" : "❌ No"}</div></div>
              <div style={{ ...s.detailItem, display: "flex", alignItems: "center", gap: "8px" }}>
                <div style={{ width: "16px", height: "16px", borderRadius: "50%", background: selectedCat.color, flexShrink: 0 }} />
                <div><div style={s.detailLabel}>Color</div><div style={{ ...s.detailVal, fontSize: "12px" }}>{selectedCat.color}</div></div>
              </div>
            </div>
            {selectedCat.notes && (
              <div style={s.notesBox}>
                <div style={s.detailLabel}>Notas 📝</div>
                <div style={{ fontSize: "14px", color: "#666", marginTop: "4px", lineHeight: 1.5 }}>{selectedCat.notes}</div>
              </div>
            )}
            {deleteConfirm === selectedCat.id ? (
              <div>
                <div style={{ textAlign: "center", color: "#e06060", fontWeight: 800, marginBottom: "12px" }}>¿Eliminar a {selectedCat.name}? 😿</div>
                <div style={s.actionRow}>
                  <button style={s.editBtn} onClick={() => setDeleteConfirm(null)}>Cancelar</button>
                  <button style={s.deleteBtn} onClick={() => deleteCat(selectedCat.id)}>Sí, eliminar</button>
                </div>
              </div>
            ) : (
              <div style={s.actionRow}>
                <button style={s.editBtn} onClick={() => openEdit(selectedCat)}>✏️ Editar</button>
                <button style={s.deleteBtn} onClick={() => setDeleteConfirm(selectedCat.id)}>🗑️ Eliminar</button>
              </div>
            )}
          </div>
        </div>
      )}

      {/* FORMULARIO */}
      {showForm && (
        <div style={s.overlay} onClick={e => e.target === e.currentTarget && setShowForm(false)}>
          <div style={s.sheet}>
            <div style={s.sheetHandle} />
            <div style={s.formTitle}>{editingCat ? "Editar gato ✏️" : "Nuevo peludo 🐾"}</div>

            <label style={s.label}>Emoji</label>
            <div style={s.emojiRow}>
              {EMOJIS.map(e => (
                <span key={e} style={{ ...s.emojiOpt, background: formData.emoji === e ? "#fff0f5" : "transparent", borderColor: formData.emoji === e ? "#ff9eb5" : "transparent" }}
                  onClick={() => setFormData(p => ({ ...p, emoji: e }))}>{e}</span>
              ))}
            </div>

            <label style={s.label}>Color</label>
            <div style={s.colorRow}>
              {COLORS.map(c => (
                <div key={c} style={{ ...s.colorOpt, background: c, borderColor: formData.color === c ? "#333" : "transparent" }}
                  onClick={() => setFormData(p => ({ ...p, color: c }))} />
              ))}
            </div>

            <label style={s.label}>Nombre *</label>
            <input style={s.input} placeholder="Ej: Luna" value={formData.name}
              onChange={e => setFormData(p => ({ ...p, name: e.target.value }))} />

            <label style={s.label}>Raza</label>
            <input style={s.input} placeholder="Ej: Siamés" value={formData.breed}
              onChange={e => setFormData(p => ({ ...p, breed: e.target.value }))} />

            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: "12px" }}>
              <div>
                <label style={s.label}>Edad (años)</label>
                <input style={s.input} type="number" min="0" placeholder="3" value={formData.age}
                  onChange={e => setFormData(p => ({ ...p, age: e.target.value }))} />
              </div>
              <div>
                <label style={s.label}>Peso</label>
                <input style={s.input} placeholder="4.2 kg" value={formData.weight}
                  onChange={e => setFormData(p => ({ ...p, weight: e.target.value }))} />
              </div>
            </div>

            <label style={s.label}>Nº Microchip</label>
            <input style={s.input} placeholder="ES123456789" value={formData.chip}
              onChange={e => setFormData(p => ({ ...p, chip: e.target.value }))} />

            <label style={s.label}>Notas</label>
            <input style={{ ...s.input, marginBottom: "16px" }} placeholder="Le encanta el sol..." value={formData.notes}
              onChange={e => setFormData(p => ({ ...p, notes: e.target.value }))} />

            <div style={s.vacRow}>
              <button style={{ ...s.toggle, background: formData.vaccinated ? "#5aab6e" : "#eee" }}
                onClick={() => setFormData(p => ({ ...p, vaccinated: !p.vaccinated }))}>
                <div style={{ width: "18px", height: "18px", borderRadius: "50%", background: "white", position: "absolute", top: "3px", left: formData.vaccinated ? "23px" : "3px", transition: "left 0.2s", boxShadow: "0 1px 4px rgba(0,0,0,0.2)" }} />
              </button>
              <span style={{ fontWeight: 800, color: "#666", fontSize: "14px" }}>Vacunado/a</span>
            </div>

            <button style={s.saveBtn} onClick={saveForm} disabled={!formData.name.trim()}>
              {editingCat ? "Guardar cambios 💾" : "Añadir gato 🐾"}
            </button>
          </div>
        </div>
      )}
    </div>
  );
}
