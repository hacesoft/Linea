[🇨🇿 **Česky**](06_PRIKLADY.md) \| [🇬🇧
English](../../en/api/06_EXAMPLES.md)

# Příklady LINEA API

## Kompletní odpověď `GET /api/v1/status`

Tento příklad zachovává kompletní strukturu datového schematu 3.
Konkrétní hodnoty jsou pouze demonstrační a citlivé či domácí
identifikátory jsou nahrazené obecnými názvy.

``` json
{
  "api": {
    "name": "LINEA API",
    "version": "1.0.0",
    "schema": 3,
    "readOnly": true
  },
  "system": {
    "timestamp": "2026-09-01T18:40:28.209Z",
    "sourceTimestamp": "2026-09-01T18:40:27.427Z",
    "ageMs": 781,
    "stale": false
  },
  "conventions": {
    "gridPower": {
      "positive": "import",
      "negative": "export",
      "unit": "W"
    },
    "batteryPower": {
      "positive": "charging",
      "negative": "discharging",
      "unit": "W"
    }
  },
  "energy": {
    "available": true,
    "pv": {
      "powerW": 0,
      "strings": [
        {
          "name": "MPPT 1",
          "powerW": 0,
          "pvVoltageV": 0.75,
          "pvCurrentA": 0,
          "yieldTodayKWh": 15.6
        },
        {
          "name": "MPPT 2",
          "powerW": 0,
          "pvVoltageV": 0.82,
          "pvCurrentA": 0,
          "yieldTodayKWh": 15.3
        }
      ]
    },
    "house": {
      "powerW": 594,
      "phases": {
        "l1PowerW": 74,
        "l2PowerW": 210,
        "l3PowerW": 310
      }
    },
    "grid": {
      "powerW": -6,
      "phases": {
        "l1PowerW": 2,
        "l2PowerW": -5,
        "l3PowerW": -3
      }
    },
    "battery": {
      "socPct": 86,
      "powerW": -696,
      "currentA": -14,
      "voltageV": 49.7,
      "batteryLifeSocLimitPct": 15
    }
  },
  "ess": {
    "available": true,
    "decision": {
      "gridPointW": 0,
      "pvSurplusW": -724,
      "predictionActive": true,
      "exportAllowed": true,
      "reason": {
        "code": "UNCLASSIFIED",
        "text": null
      }
    },
    "switches": {
      "controlModeEssAcGrid": true,
      "spotGridCharging": false,
      "gridCharging": false,
      "gridConsumption": true,
      "energyThresholdInjector": true,
      "nonBatteryPriority": false,
      "delayCharging": true,
      "dynamicSocReserve": false,
      "predictionThreshold": true,
      "socDeltaBeforeExport": false,
      "morningPeakBatterySales": false,
      "eveningPeakBatterySales": false
    },
    "settings": {
      "balancingReserveW": 130,
      "setGridValueW": -100,
      "maxGridPointW": -4000,
      "spotThresholdPrice": 1.04,
      "morningSocSalesPct": 38,
      "eveningSocSalesPct": 95,
      "gridChargingSocPct": 90,
      "predictionThresholdKWh": 22,
      "socDeltaBeforeExportPct": 35,
      "chargingDurationGridH": 10,
      "acceptablePriceGrid": 2.01
    },
    "time": {
      "delayCharging": {
        "start": "04:30",
        "stop": "11:00",
        "startMs": 16200000,
        "stopMs": 39600000
      },
      "morningPeakHours": [
        7,
        8
      ],
      "eveningPeakHours": [
        20,
        21
      ]
    }
  },
  "spot": {
    "available": true,
    "currentPrice": 5.52
  },
  "forecast": {
    "available": true,
    "solarYieldForecastKWh": 28.7,
    "consumptionForecastKWh": 19.02
  },
  "solar": {
    "available": true,
    "sunrise": "06:08:31",
    "sunset": "19:38:52",
    "dayLength": "13h 30m 21s"
  },
  "weather": {
    "available": true,
    "today": "Zataženo",
    "rainProbabilityPct": 3,
    "trend": "stejné"
  },
  "vrm": {
    "available": true,
    "data": {
      "updatedAt": "2026-09-01T18:40:07.199Z",
      "source": "Victron VRM live_feed + LINEA battery counters",
      "today": {
        "pvYieldKWh": 30.9,
        "consumptionKWh": 18.95,
        "gridImportKWh": 1.11,
        "gridExportKWh": 8.26,
        "batteryChargeKWh": 38.92,
        "batteryDischargeKWh": 39.0
      }
    }
  },
  "temperatures": {
    "available": true,
    "data": {
      "updatedAt": "2026-09-01T18:40:14.458Z",
      "racks": [
        {
          "name": "Rack",
          "temperatureC": 28.76
        }
      ],
      "inverters": [
        {
          "name": "Měnič L1",
          "temperatureC": 31.9
        }
      ],
      "other": []
    }
  },
  "shelly": {
    "available": true,
    "data": {
      "updatedAt": "2026-09-01T18:40:00.267Z",
      "devices": [
        {
          "name": "Bojler",
          "kind": "output",
          "channel": 0,
          "state": false,
          "available": true
        }
      ],
      "smokeDetectors": [
        {
          "name": "Kouřové čidlo",
          "alarm": false,
          "ok": true,
          "batteryPct": 92,
          "batteryVoltageV": 2.97,
          "rssiDbm": -27,
          "wakeupReason": "timer",
          "lastSeen": "2026-09-01T12:10:39.188Z",
          "ageSec": 10348
        }
      ]
    }
  },
  "ups": {
    "available": true,
    "data": {
      "updatedAt": "2026-09-01T18:40:25.400Z",
      "name": "UPS",
      "online": true,
      "onBattery": false,
      "status": {
        "raw": "OL",
        "lowBattery": false,
        "charging": false,
        "discharging": false,
        "overload": false,
        "replaceBattery": false,
        "bypass": false
      },
      "battery": {
        "chargePct": 100,
        "voltageV": null,
        "runtimeSec": 2382
      },
      "input": {
        "voltageV": 231
      },
      "output": {
        "voltageV": 234,
        "frequencyHz": 50
      },
      "load": {
        "percent": 13,
        "realPowerW": 57
      }
    }
  },
  "climate": {
    "available": true,
    "data": {
      "updatedAt": "2026-09-01T18:39:53.551Z",
      "devices": [
        {
          "name": "Místnost",
          "cloudUp": true,
          "on": false,
          "operationMode": "cooling",
          "roomTemperatureC": 25,
          "outdoorTemperatureC": 17.5,
          "setpointC": 18,
          "energy": {
            "unit": "kWh",
            "todayKWh": 2.4,
            "weekKWh": 5.7,
            "monthKWh": 397.8,
            "coolingMonthKWh": 396.2,
            "heatingMonthKWh": 1.6
          },
          "error": false,
          "errorCode": "00-",
          "firmwareVersion": "2_6_2",
          "firmwareChanged": false
        }
      ]
    }
  }
}
```

Význam každé položky, datový typ, jednotka a znaménková konvence jsou
popsány v [Datovém modelu -- schema 1](04_DATOVY_MODEL.md).

[← LINEA API](PREHLED.md)

## Příklad SPOT cen

```json
{
  "spot": {
    "available": true,
    "currentPrice": 4.12,
    "intervalMinutes": 60,
    "today": {
      "date": "2026-09-05",
      "available": true,
      "prices": [
        {"hour": 0, "price": 2.31},
        {"hour": 1, "price": 2.12}
      ]
    },
    "tomorrow": {
      "date": "2026-09-06",
      "available": true,
      "prices": [
        {"hour": 0, "price": 1.98},
        {"hour": 1, "price": 1.87}
      ]
    }
  }
}
```

## Příklad VRM predikce pro graf

```json
{
  "forecast": {
    "available": true,
    "source": "Victron VRM",
    "intervalMinutes": 15,
    "solarYieldForecastKWh": 25.33,
    "consumptionForecastKWh": 20.64,
    "series": {
      "solarYield": [
        {
          "timestampMs": 1788595200000,
          "timestamp": "2026-09-05T08:00:00.000Z",
          "energyKWh": 0.18
        }
      ],
      "consumption": [
        {
          "timestampMs": 1788595200000,
          "timestamp": "2026-09-05T08:00:00.000Z",
          "energyKWh": 0.21
        }
      ]
    }
  }
}
```

