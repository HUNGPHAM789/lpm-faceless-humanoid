---
name: humanoid-replaceable-assets
description: Layer assets that ATTACH to the low-poly humanoid base (hair, hats, shoes, gloves, clothes, backpacks) via its sockets/contract. Open before building any hair/shoe/garment for LPM or the combined humanoid. Hair recipe proven 22/10/2026; hair STYLIZATION still under development.
version: 0.2.0
metadata:
  hermes:
    tags: [humanoid, replaceable-assets, hair, low-poly, sockets]
    related_skills: [low-poly-origami-character]
---

# Replaceable humanoid assets (layer skill)

Base body contract = the humanoid skill (`low-poly-origami-character`, §4): named parts + `SOCKET_*` (dau, ban_tay_*, cam_*, ban_chan_*, lung, hong_*).
Rule: an asset is its own mesh/GLB, authored in the socket's space, never merged into the body.

## Hair — recipe that WORKS (test 22/10/2026, simplified-spec agent)
- ✅ Build the hair cap ON THE HEAD'S OWN VERTEX RINGS above the brow line, then scale it about `HAIR_SOCKET` by **k ≈ 1.032** (≈3 % out).
  Result: 0 / 4,000 sample points inside the head, no floating; median gap 3.9 mm.
- ⛔ An independently modeled cap with a fixed 6 mm offset: **106 / 4,000** samples ended inside the faceted head — a large-facet cap can't follow a faceted scalp.
- Detailed-spec agent also fitted its short hair at 0 verts inside (min gap 0.94 mm) by authoring in socket space against the actual head.
- `HAIR_SOCKET`: skull centre at brow height (≈ chin + 0.12 m for a 1.65 m body); hair parented to it at identity.
- Cap underside stays OPEN (seats over the scalp); hair is a separate node; swapping hair keeps the socket transform.

## Hair — NOT solved yet (Boss 22/10: «làm tóc vẫn chưa ra stylized cần phát triển thêm»)
Both test hairs read as a helmet/cap, not stylized folded clumps. Next development:
- clump grammar: N sectors around the head × crown rows; hanging rows only at sides/back; fringe stops above the brow; each clump = folded wedge with a pointed tip, alternating lengths;
- silhouette gate: % of hairline perimeter broken by clump tips; tip count per 90° sector; no clipping (keep the 4,000-sample inside test).

## Shoes / gloves / garments
- Shoes: remove `ban_chan_*` and place the shoe at `SOCKET_ban_chan_*` (the shin is closed, no hole).
- Garments: reuse the body section table (`THAN`) scaled outward 1–2 cm over the garment's z-range (shirt = torso rings × 1.03–1.05).
- Hand-held props: align the handle with `cam_*` (Ø32 mm grip axis) and build the body with `--co 1`.

## v0.2 (22/10/2026) — simple hair + settler outfits for the DETAILED humanoid (built, gated)
Boss: «Giờ tóc đơn giản cho nam và nữ. Đồ đơn giản cho nam và nữ — đồ thời nào là dễ làm RTS settler nhất? Giống GodUs?»
Era chosen: EARLY VILLAGE SETTLER (tribal → early medieval peasant) — garments are offset shells of the body tables, no buttons/collars; readable at isometric size by colour (Godus-like).
Code: `~/projects/GameDev/lpd_chi_tiet/toc_do.py` (`GIOI=nam|nu python3 toc_do.py out_<g>`) → HAIR_SHORT_M / HAIR_BOB_F + OUTFIT_SETTLER_M (ao_than, ao_vat, quan, that_lung, giay) / OUTFIT_SETTLER_F (vay_than, vay_duoi, that_lung, giay). Render: `xem_mac_do.py`.
- HAIR: grid on the head's 16 columns; place every vertex by RAY-CASTING the real head mesh (incl. ears) from HAIR_SOCKET + gap 11 mm (+4 mm in the drop zone, +6 mm on columns next to the fringe). ⛔ Don't place hair on the interpolated scalp envelope: the real crown sits 8 mm behind it ⇒ 147/4000 clipping. ⛔ Gap 4 mm: the face between two hair vertices is a chord and cuts into bulges. Fringe hem above the brow; male hem above the ears until the 130° column (earlier switch ⇒ the slanted face crosses the ear back); bob = horizontal rays at ear level, slight puff, jagged hem.
- GARMENTS: shell = body faces of chosen regions offset along vertex normals (torso 10–11 mm, legs 8, feet 7); tunic hem / skirt = 16-sided tube whose radius per direction = farthest DENSE SURFACE SAMPLE of torso+legs at that z + gap, flaring down. ⛔ Measure from surface samples, not vertices (gap between thigh ring 0.765 and hip ring 0.865 ⇒ tube shrank inside body, 755/2000 clipping). Tunic hem must stay above the fingertips (0.716 m) or hands pierce it.
- GATE (in toc_do.py, exit 1): hair 0/4000 samples inside body; each garment ≤2 % of 2000 samples inside. Inside test = parity with 3 jittered rays, majority vote — a single +X ray runs along ring planes, hits edges, and mis-reported 4/8 known-inside points (calibrated 0/56 outside, 0/8 inside after the fix).
- Result: hair 368 tris; male outfit 1,329 tris; female 953 tris; all gates pass. Hair still reads simple (not folded-clump stylized).


## v0.3 (22/10/2026) — hand tools for settlers (Boss: berry · bow for deer · axe · pickaxe · hoe · shovel · watering can · fishing rod)
- Build: `python3 scripts/cong_cu.py <dir>` → one GLB per tool + `cong_cu.json`; gate runs at the end, exit 1 on fail.
  Output v1: `~/projects/GameDev/assets/tools/settler_v1/` (axe 40 tris … basket 268; all ≤600).
- Route: convex-hull primitives (`khoi`, `ong` prism, `hop`) — hard objects, NOT the organic skill (that makes clay blobs).
- **Grip contract**: origin = centre of the grip; handle axis = local +Z; working side (blade/spout/head) = local +X.
  Matches LPM `--co 1` socket `cam_L/cam_R` (`vi_tri`, `len` = cylinder axis, Ø32 mm).
  To attach: origin → `vi_tri`, +Z → `len`, +X → chosen world direction (axe/pick/hoe/shovel blade DOWN; can/basket body hangs, so +X = UP).
- Gates (measured on the mesh, not read from code): every part watertight · ≤600 tris · handle radius at the origin
  13–17 mm via a plane section at z=0. Reverse test 22/10: fishing rod tapered to 12.8 mm → rc=1; fixed to 14.5 → rc=0.
- Render: `blender -b -P scripts/xem_cong_cu.py -- <dir> <lpm.glb> <lpm.json> <out.jpg> R=riu L=binh_tuoi` (lineup + 3/4 figure).
- Traps: watering can / basket hang on −X, so the spout must rise toward +X (first version pointed it at the ground);
  a hoe blade facing the camera reads as a hook — make it big (~17 cm) and angled ~70°.
- Not done: two-handed poses (bow draw, hoe swing) need a second socket on the tool + IK; no clipping check vs body yet.
- **Review on the APPROVED settler** (Boss 22/10 «use our finished settlers model»): `CO=1 bash lpd_chi_tiet/build.sh nam|nu`
  (grip hands, gate soi_cam.py) then `scripts/xem_settler_cam.py`. xem_cong_cu.py refuses a bare body.
- v0.4 (Boss 22/10): colours stay raw sRGB (house pale palette) — Boss tried Kiln's warm linear colours, then «tôi thích màu bên mình hơn».
  Pickaxe = crescent head. Rod = 6 segments × 4°; gate ⑦ fails any joint > 8° (old 25° kink → blocked).

## Hanging a tool on a peg / rack (Boss 23/10/2026: «Physics ko đúng»)
Grip on top of the peg, grip axis along the peg, body hanging straight down (centre of mass under the grip, ≤ 3 cm sideways). Never place a
hung tool by yaw only. Reference implementation + gate: `~/projects/GameDev/lpms/assets/gia_vu_khi.py` (weapon rack, LPMS README §Hanging).
