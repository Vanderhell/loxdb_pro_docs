# loxdb_alerting

`loxdb_alerting` je Pro bridge modul pre alerty nad metrikami `loxdb_metrics`.

## Čo robí

- vyhodnocuje 3 zdroje rizika:
  - `near_full_risk_pct`
  - `wal_fill_pct`
  - `selfcheck anomalies` (kv+ts+rel)
- drží alert stavový automat:
  - `OK -> PENDING -> FIRING -> RESOLVED`
- pri `WARN` používa `pending_cycles` (anti-flap)
- pri `FIRING` používa cooldown/rate-limit cez `microres`
- threshold logiku deleguje na `microhealth`

## Závislosti

- `microhealth` (`mhealth.h`) na threshold/severity engine
- `microres` (`mres.h`) na cooldown token-bucket

## Rýchle použitie

```c
static uint32_t app_clock_ms(void) { return hal_millis(); }

static void on_alert(const loxdb_alerting_event_t *ev, void *ctx) {
    (void)ctx;
    // napr. microbus publish / microlog zapis
}

loxdb_alerting_cfg_t cfg = {0};
cfg.clock_ms = app_clock_ms;
cfg.emit_cb = on_alert;
cfg.policy.risk_warn_pct = 70;
cfg.policy.risk_critical_pct = 90;
cfg.policy.wal_warn_pct = 80;
cfg.policy.wal_critical_pct = 95;
cfg.policy.anomaly_warn_count = 1;
cfg.policy.anomaly_critical_count = 5;
cfg.policy.pending_cycles = 2;
cfg.policy.cooldown_ms = 1000;

loxdb_alerting_t *alerting = loxdb_alerting_create(&cfg);

loxdb_metrics_snapshot_t snap;
loxdb_metrics_collect(db, &snap);
loxdb_alerting_event_t ev;
loxdb_alerting_eval(alerting, &snap, &ev);
loxdb_alerting_destroy(alerting);
```
