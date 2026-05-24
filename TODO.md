# Slice_GDHB TODO

## CI Results (Pass 1)

- [x] DRC: FAIL — 6 errors (all zones_intersect: copper zones with same priority intersecting)
- [x] ERC: FAIL — 1 error (power_pin_not_driven: input power pin not driven by any output power pins)
- [x] Fab: FAIL (blocked by ERC error)
- [ ] gen-kibot-index: SKIPPED (upstream failed)
- [ ] deploy-pages: SKIPPED (upstream failed)

**Note: 4-layer board (In1.Cu/In2.Cu enabled in kibot config)**

## Pass 2 – Pre-fab Review

- [ ] Fix 6x zone intersect errors (set distinct zone priorities)
- [ ] Fix 1x power_pin_not_driven ERC error
- [ ] Verify BOM completeness
- [ ] Confirm board outline and mounting holes
- [ ] Update README.md (still says "Slice Template")
