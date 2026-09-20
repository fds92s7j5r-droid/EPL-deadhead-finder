# Engineer Pay Log — Deadhead Finder Sandbox v3.2.1

## Practical WSY ranking cleanup

This build keeps all v3.2.0 behavior and changes how multiple possible West Side Yard intercepts are presented.

### New rule
For practical WSY options:
1. Find the fastest possible WSY-origin route.
2. Find the best **direct** WSY-origin route.
3. If the direct route arrives within **30 minutes** of the fastest practical route, recommend the direct train.
4. If a transfer-heavy route is faster, keep it visible as **Faster possible WSY alternative**.
5. Always keep the **Fully protected route home** separate.

### Job 108 / Babylon expected behavior
The intended presentation is now:
- **Recommended practical WSY route:** Train 102 direct from West Side Yard → Babylon.
- **Faster possible WSY alternative:** e.g. 2306 → Train 2 at Jamaica if that remains the fastest current schedule option.
- **Fully protected route home:** first paper-legal Penn option after release + 20 minutes.

All practical WSY options remain explicitly **NOT PROTECTED** and **not guaranteed**.

### GTFS cache
The v3.2.0 cache test passed and the IndexedDB persistence code is unchanged.
