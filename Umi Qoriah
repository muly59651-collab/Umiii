import { useState, useMemo } from "react";

const INITIAL_PATIENTS = [
  { id: 1, name: "Budi Santoso", doctor: "dr. Andini Putri", service: "Rawat Jalan", total: 250000, status: "Lunas", method: "Tunai", date: "19 Mei 2026" },
  { id: 2, name: "Siti Aisyah", doctor: "dr. Rahmat Hidayat", service: "UGD", total: 550000, status: "Belum Bayar", method: "-", date: "20 Mei 2026" },
  { id: 3, name: "Agus Saputra", doctor: "dr. Nabila Hanum", service: "Rawat Inap", total: 1250000, status: "Lunas", method: "Transfer Bank", date: "21 Mei 2026" },
  { id: 4, name: "Dewi Rahayu", doctor: "dr. Fajar Nugroho", service: "Laboratorium", total: 185000, status: "Belum Bayar", method: "-", date: "22 Mei 2026" },
  { id: 5, name: "Hendra Wijaya", doctor: "dr. Andini Putri", service: "Rawat Jalan", total: 320000, status: "Lunas", method: "QRIS", date: "23 Mei 2026" },
];

const SERVICE_PRICES = {
  "Rawat Jalan": 250000,
  "Rawat Inap": 1250000,
  "UGD": 550000,
  "Laboratorium": 185000,
};

const ICONS = {
  hospital: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-6 h-6">
      <path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/>
    </svg>
  ),
  users: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-5 h-5">
      <path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 00-3-3.87"/><path d="M16 3.13a4 4 0 010 7.75"/>
    </svg>
  ),
  check: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" className="w-5 h-5">
      <polyline points="20 6 9 17 4 12"/>
    </svg>
  ),
  money: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-5 h-5">
      <rect x="1" y="4" width="22" height="16" rx="2" ry="2"/><line x1="1" y1="10" x2="23" y2="10"/>
    </svg>
  ),
  search: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" className="w-4 h-4">
      <circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/>
    </svg>
  ),
  plus: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" className="w-4 h-4">
      <line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/>
    </svg>
  ),
  trash: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-4 h-4">
      <polyline points="3 6 5 6 21 6"/><path d="M19 6l-1 14H6L5 6"/><path d="M10 11v6"/><path d="M14 11v6"/><path d="M9 6V4h6v2"/>
    </svg>
  ),
  edit: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-4 h-4">
      <path d="M11 4H4a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 013 3L12 15l-4 1 1-4 9.5-9.5z"/>
    </svg>
  ),
  close: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" className="w-5 h-5">
      <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
    </svg>
  ),
  receipt: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-5 h-5">
      <path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/><polyline points="10 9 9 9 8 9"/>
    </svg>
  ),
  chart: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" className="w-5 h-5">
      <line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/><line x1="2" y1="20" x2="22" y2="20"/>
    </svg>
  ),
};

const fmt = (n) => `Rp ${n.toLocaleString("id-ID")}`;

const today = () => {
  const d = new Date();
  const months = ["Jan","Feb","Mar","Apr","Mei","Jun","Jul","Agu","Sep","Okt","Nov","Des"];
  return `${d.getDate()} ${months[d.getMonth()]} ${d.getFullYear()}`;
};

const EMPTY_FORM = { name: "", doctor: "", service: "Rawat Jalan", total: SERVICE_PRICES["Rawat Jalan"], method: "Tunai", status: "Belum Bayar" };

export default function HospitalCashierApp() {
  const [patients, setPatients] = useState(INITIAL_PATIENTS);
  const [search, setSearch] = useState("");
  const [filterStatus, setFilterStatus] = useState("Semua");
  const [form, setForm] = useState(EMPTY_FORM);
  const [editingId, setEditingId] = useState(null);
  const [showForm, setShowForm] = useState(false);
  const [toast, setToast] = useState(null);
  const [activeTab, setActiveTab] = useState("dashboard");
  const [deleteConfirm, setDeleteConfirm] = useState(null);

  const showToast = (msg, type = "success") => {
    setToast({ msg, type });
    setTimeout(() => setToast(null), 3000);
  };

  const filtered = useMemo(() => {
    return patients.filter((p) => {
      const matchSearch = p.name.toLowerCase().includes(search.toLowerCase()) ||
        p.doctor.toLowerCase().includes(search.toLowerCase()) ||
        p.service.toLowerCase().includes(search.toLowerCase());
      const matchStatus = filterStatus === "Semua" || p.status === filterStatus;
      return matchSearch && matchStatus;
    });
  }, [patients, search, filterStatus]);

  const stats = useMemo(() => ({
    total: patients.length,
    lunas: patients.filter((p) => p.status === "Lunas").length,
    belumBayar: patients.filter((p) => p.status === "Belum Bayar").length,
    pendapatan: patients.filter((p) => p.status === "Lunas").reduce((s, p) => s + p.total, 0),
    piutang: patients.filter((p) => p.status === "Belum Bayar").reduce((s, p) => s + p.total, 0),
  }), [patients]);

  const serviceStats = useMemo(() => {
    const map = {};
    patients.forEach((p) => {
      if (!map[p.service]) map[p.service] = { count: 0, total: 0 };
      map[p.service].count++;
      map[p.service].total += p.total;
    });
    return Object.entries(map).map(([name, v]) => ({ name, ...v }));
  }, [patients]);

  const handleFormChange = (e) => {
    const { name, value } = e.target;
    setForm((prev) => {
      const updated = { ...prev, [name]: value };
      if (name === "service") updated.total = SERVICE_PRICES[value] || 0;
      return updated;
    });
  };

  const handleSubmit = () => {
    if (!form.name.trim() || !form.doctor.trim()) {
      showToast("Nama pasien dan dokter wajib diisi!", "error");
      return;
    }
    if (editingId !== null) {
      setPatients((prev) => prev.map((p) => p.id === editingId ? { ...p, ...form, total: Number(form.total) } : p));
      showToast("Data pasien berhasil diperbarui!");
    } else {
      const newId = Math.max(...patients.map((p) => p.id), 0) + 1;
      setPatients((prev) => [...prev, { id: newId, ...form, total: Number(form.total), date: today() }]);
      showToast("Pasien baru berhasil ditambahkan!");
    }
    setForm(EMPTY_FORM);
    setEditingId(null);
    setShowForm(false);
  };

  const handleEdit = (patient) => {
    setForm({ name: patient.name, doctor: patient.doctor, service: patient.service, total: patient.total, method: patient.method === "-" ? "Tunai" : patient.method, status: patient.status });
    setEditingId(patient.id);
    setShowForm(true);
    setActiveTab("dashboard");
  };

  const handleDelete = (id) => {
    setPatients((prev) => prev.filter((p) => p.id !== id));
    setDeleteConfirm(null);
    showToast("Data pasien berhasil dihapus!", "error");
  };

  const toggleStatus = (id) => {
    setPatients((prev) => prev.map((p) => p.id === id ? { ...p, status: p.status === "Lunas" ? "Belum Bayar" : "Lunas", method: p.status === "Lunas" ? "-" : "Tunai" } : p));
    showToast("Status pembayaran diperbarui!");
  };

  const maxBar = Math.max(...serviceStats.map((s) => s.total), 1);

  return (
    <div style={{ fontFamily: "'DM Sans', 'Segoe UI', sans-serif", background: "#f0f4f8", minHeight: "100vh" }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=Space+Grotesk:wght@500;600;700&display=swap');
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { background: #f0f4f8; }
        .card { background: #fff; border-radius: 20px; border: 1px solid #e8edf2; box-shadow: 0 2px 12px rgba(0,0,0,0.05); }
        .btn-primary { background: linear-gradient(135deg, #1a56db, #0ea5e9); color: #fff; border: none; border-radius: 12px; padding: 10px 20px; font-weight: 600; cursor: pointer; display: flex; align-items: center; gap: 6px; transition: opacity .2s, transform .1s; font-size: 14px; }
        .btn-primary:hover { opacity: .9; transform: translateY(-1px); }
        .btn-ghost { background: transparent; border: 1.5px solid #e2e8f0; border-radius: 10px; padding: 7px 10px; cursor: pointer; display: flex; align-items: center; color: #64748b; transition: all .2s; font-size: 13px; }
        .btn-ghost:hover { background: #f1f5f9; color: #1e293b; }
        .input { width: 100%; border: 1.5px solid #e2e8f0; border-radius: 12px; padding: 10px 14px; font-size: 14px; outline: none; transition: border .2s; font-family: inherit; background: #fafbfc; color: #1e293b; }
        .input:focus { border-color: #1a56db; background: #fff; box-shadow: 0 0 0 3px rgba(26,86,219,0.08); }
        .badge { padding: 4px 12px; border-radius: 20px; font-size: 12px; font-weight: 600; display: inline-block; }
        .badge-lunas { background: #dcfce7; color: #16a34a; }
        .badge-pending { background: #fef3c7; color: #d97706; }
        .tab { padding: 8px 18px; border-radius: 10px; cursor: pointer; font-weight: 500; font-size: 14px; transition: all .2s; border: none; background: transparent; color: #64748b; display: flex; align-items: center; gap: 7px; }
        .tab.active { background: #fff; color: #1a56db; box-shadow: 0 2px 8px rgba(0,0,0,0.08); }
        .tab:hover:not(.active) { background: rgba(255,255,255,0.5); color: #1e293b; }
        tr:hover td { background: #f8fafc; }
        .modal-overlay { position: fixed; inset: 0; background: rgba(15,23,42,0.5); backdrop-filter: blur(4px); z-index: 50; display: flex; align-items: center; justify-content: center; padding: 20px; }
        .modal { background: #fff; border-radius: 24px; padding: 28px; width: 100%; max-width: 460px; box-shadow: 0 24px 60px rgba(0,0,0,0.15); }
        .toast { position: fixed; bottom: 24px; right: 24px; z-index: 100; padding: 14px 20px; border-radius: 14px; font-weight: 500; font-size: 14px; box-shadow: 0 8px 24px rgba(0,0,0,0.15); animation: slideUp .3s ease; }
        .toast-success { background: #1a56db; color: #fff; }
        .toast-error { background: #ef4444; color: #fff; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        .stat-card { transition: transform .2s; }
        .stat-card:hover { transform: translateY(-2px); }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 3px; }
      `}</style>

      {/* Sidebar */}
      <div style={{ display: "flex", minHeight: "100vh" }}>
        <aside style={{ width: 220, background: "#0f172a", display: "flex", flexDirection: "column", padding: "24px 16px", position: "sticky", top: 0, height: "100vh", flexShrink: 0 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 10, marginBottom: 36, paddingLeft: 8 }}>
            <div style={{ background: "linear-gradient(135deg,#1a56db,#0ea5e9)", borderRadius: 12, padding: 8, color: "#fff" }}>{ICONS.hospital}</div>
            <div>
              <div style={{ color: "#fff", fontWeight: 700, fontSize: 13, fontFamily: "'Space Grotesk', sans-serif" }}>MediKas</div>
              <div style={{ color: "#64748b", fontSize: 11 }}>Sistem Kasir RS</div>
            </div>
          </div>

          {[
            { id: "dashboard", label: "Dashboard", icon: ICONS.chart },
            { id: "patients", label: "Data Pasien", icon: ICONS.users },
            { id: "transactions", label: "Transaksi", icon: ICONS.receipt },
          ].map((item) => (
            <button key={item.id} onClick={() => setActiveTab(item.id)}
              style={{ display: "flex", alignItems: "center", gap: 10, padding: "11px 14px", borderRadius: 12, border: "none", cursor: "pointer", marginBottom: 6, fontWeight: 500, fontSize: 14, fontFamily: "inherit", background: activeTab === item.id ? "rgba(26,86,219,0.2)" : "transparent", color: activeTab === item.id ? "#60a5fa" : "#94a3b8", transition: "all .2s" }}>
              {item.icon}{item.label}
            </button>
          ))}

          <div style={{ marginTop: "auto", borderTop: "1px solid #1e293b", paddingTop: 16 }}>
            <div style={{ color: "#475569", fontSize: 12, paddingLeft: 8 }}>
              <div style={{ marginBottom: 4 }}>RS Medika Husada</div>
              <div style={{ color: "#334155" }}>{today()}</div>
            </div>
          </div>
        </aside>

        {/* Main Content */}
        <main style={{ flex: 1, padding: "28px 28px 40px", overflow: "auto" }}>
          {/* Header */}
          <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 28 }}>
            <div>
              <h1 style={{ fontFamily: "'Space Grotesk', sans-serif", fontSize: 22, fontWeight: 700, color: "#0f172a" }}>
                {activeTab === "dashboard" && "Dashboard Kasir"}
                {activeTab === "patients" && "Manajemen Pasien"}
                {activeTab === "transactions" && "Riwayat Transaksi"}
              </h1>
              <p style={{ color: "#64748b", fontSize: 14, marginTop: 2 }}>Selamat datang di sistem kasir Rumah Sakit Medika Husada</p>
            </div>
            <button className="btn-primary" onClick={() => { setShowForm(true); setForm(EMPTY_FORM); setEditingId(null); }}>
              {ICONS.plus} Tambah Pasien
            </button>
          </div>

          {/* DASHBOARD TAB */}
          {activeTab === "dashboard" && (
            <>
              {/* Stat Cards */}
              <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(180px, 1fr))", gap: 16, marginBottom: 24 }}>
                {[
                  { label: "Total Pasien", value: stats.total, sub: "terdaftar hari ini", icon: ICONS.users, accent: "#1a56db", bg: "#eff6ff" },
                  { label: "Lunas", value: stats.lunas, sub: `dari ${stats.total} pasien`, icon: ICONS.check, accent: "#16a34a", bg: "#f0fdf4" },
                  { label: "Belum Bayar", value: stats.belumBayar, sub: "perlu diproses", icon: ICONS.receipt, accent: "#d97706", bg: "#fffbeb" },
                  { label: "Total Pendapatan", value: fmt(stats.pendapatan), sub: "sudah diterima", icon: ICONS.money, accent: "#0ea5e9", bg: "#f0f9ff", wide: true },
                  { label: "Piutang", value: fmt(stats.piutang), sub: "belum dibayar", icon: ICONS.money, accent: "#ef4444", bg: "#fef2f2", wide: true },
                ].map((s, i) => (
                  <div key={i} className="card stat-card" style={{ padding: "20px 22px" }}>
                    <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
                      <div>
                        <div style={{ color: "#64748b", fontSize: 13, marginBottom: 10 }}>{s.label}</div>
                        <div style={{ fontFamily: "'Space Grotesk', sans-serif", fontSize: typeof s.value === "string" ? 18 : 30, fontWeight: 700, color: s.accent }}>{s.value}</div>
                        <div style={{ color: "#94a3b8", fontSize: 12, marginTop: 4 }}>{s.sub}</div>
                      </div>
                      <div style={{ background: s.bg, color: s.accent, padding: 10, borderRadius: 12 }}>{s.icon}</div>
                    </div>
                  </div>
                ))}
              </div>

              {/* Service Bar Chart */}
              <div className="card" style={{ padding: 24, marginBottom: 24 }}>
                <h2 style={{ fontFamily: "'Space Grotesk', sans-serif", fontWeight: 700, fontSize: 16, color: "#0f172a", marginBottom: 20 }}>Pendapatan per Layanan</h2>
                <div style={{ display: "flex", flexDirection: "column", gap: 14 }}>
                  {serviceStats.map((s, i) => (
                    <div key={i}>
                      <div style={{ display: "flex", justifyContent: "space-between", fontSize: 13, marginBottom: 6 }}>
                        <span style={{ fontWeight: 500, color: "#1e293b" }}>{s.name}</span>
                        <span style={{ color: "#64748b" }}>{fmt(s.total)} &bull; {s.count} pasien</span>
                      </div>
                      <div style={{ background: "#f1f5f9", borderRadius: 8, height: 10, overflow: "hidden" }}>
                        <div style={{ height: "100%", borderRadius: 8, background: ["#1a56db","#0ea5e9","#16a34a","#d97706"][i % 4], width: `${(s.total / maxBar) * 100}%`, transition: "width 1s ease" }} />
                      </div>
                    </div>
                  ))}
                </div>
              </div>

              {/* Recent Transactions */}
              <div className="card" style={{ padding: 24 }}>
                <h2 style={{ fontFamily: "'Space Grotesk', sans-serif", fontWeight: 700, fontSize: 16, color: "#0f172a", marginBottom: 20 }}>Transaksi Terbaru</h2>
                <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(240px, 1fr))", gap: 14 }}>
                  {patients.slice(-4).reverse().map((p) => (
                    <div key={p.id} style={{ border: "1.5px solid #e8edf2", borderRadius: 16, padding: "16px 18px", background: "#fafbfc" }}>
                      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 10 }}>
                        <div>
                          <div style={{ fontWeight: 600, color: "#0f172a", fontSize: 14 }}>{p.name}</div>
                          <div style={{ color: "#64748b", fontSize: 12, marginTop: 2 }}>{p.service} &bull; {p.date}</div>
                        </div>
                        <span className={`badge ${p.status === "Lunas" ? "badge-lunas" : "badge-pending"}`}>{p.status}</span>
                      </div>
                      <div style={{ fontFamily: "'Space Grotesk', sans-serif", fontWeight: 700, fontSize: 18, color: p.status === "Lunas" ? "#16a34a" : "#d97706" }}>{fmt(p.total)}</div>
                    </div>
                  ))}
                </div>
              </div>
            </>
          )}

          {/* PATIENTS TAB */}
          {activeTab === "patients" && (
            <div className="card" style={{ padding: 24 }}>
              <div style={{ display: "flex", gap: 12, marginBottom: 20, flexWrap: "wrap" }}>
                <div style={{ position: "relative", flex: 1, minWidth: 200 }}>
                  <span style={{ position: "absolute", left: 12, top: "50%", transform: "translateY(-50%)", color: "#94a3b8" }}>{ICONS.search}</span>
                  <input className="input" placeholder="Cari nama, dokter, layanan..." value={search} onChange={(e) => setSearch(e.target.value)} style={{ paddingLeft: 36 }} />
                </div>
                {["Semua", "Lunas", "Belum Bayar"].map((f) => (
                  <button key={f} className="btn-ghost" onClick={() => setFilterStatus(f)}
                    style={{ background: filterStatus === f ? "#eff6ff" : "", color: filterStatus === f ? "#1a56db" : "", borderColor: filterStatus === f ? "#bfdbfe" : "" }}>
                    {f} {f !== "Semua" && <span style={{ background: filterStatus === f ? "#1a56db" : "#e2e8f0", color: filterStatus === f ? "#fff" : "#64748b", borderRadius: 20, padding: "1px 7px", fontSize: 11, marginLeft: 4, fontWeight: 600 }}>{f === "Lunas" ? stats.lunas : stats.belumBayar}</span>}
                  </button>
                ))}
