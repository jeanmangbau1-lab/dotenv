@st.cache_data(show_spinner=False, ttl=3600)
def _recents_allsports():
    try:
        cle = st.secrets["SPORTS_API_KEY"]
    except Exception:
        return []
    try:
        fin = date.today()
        deb = fin - pd.Timedelta(days=14)
        r = requests.get(
            "https://apiv2.allsportsapi.com/football/",
            params={"met": "Fixtures", "APIkey": cle,
                    "from": deb.isoformat(), "to": fin.isoformat()},
            timeout=10)
        r.raise_for_status()
        out = []
        for e in r.json().get("result") or []:
            sc = re.findall(r"\d+", e.get("event_final_result") or "")
            if len(sc) != 2:
                continue
            out.append((e["event_date"], e["event_home_team"],
                        e["event_away_team"], int(sc[0]), int(sc[1])))
        return out
    except Exception:
        return []
for dt, h, a, hg, ag in _recents_fd(code) + _recents_tsdb(code):
SPORTS_API_KEY = "votre_clé_allsports"
