# RASED — Brand Notes

## The idea (logo v2 design, renamed RASED on 14 Nov 2026)

**RASED** (راصد) means *observer*. The mark is a **radar scope**: an outer ring, a blue sweep covering the first 60° from north, a faint inner range ring, the radar at the centre and one contact (a ship) on the inner ring. It says "we watch the approaches and see ships coming" in one shape. The wordmark is **RASED** in Outfit SemiBold capitals.

## Colors

| Name | Hex | Use |
|---|---|---|
| Navy | `#121C2C` | Mark and wordmark on light backgrounds; dark backgrounds |
| Signal blue | `#0F5C8A` | The radar sweep (the one accent); blue backgrounds; document headings |
| Mist | `#E1E5EC` | Inner range ring on light backgrounds |
| Light | `#F4F5F9` | Neutral panels, table zebra rows |
| White | `#FFFFFF` | Mark and wordmark on navy or blue (sweep at 42% white, inner ring at 25% white) |

## Typography

- **Outfit** SemiBold (600) for the wordmark. Free under the SIL Open Font License; the file used to build the logo is `build/fonts/Outfit-VF.ttf` (not committed).
- Documents: Calibri or any clean sans-serif for body text.

## Files (`brand/`)

| File | Use |
|---|---|
| `rased_logo.svg`, `.png` | **Main logo**: mark above RASED, for white or light backgrounds (cover pages, title slides) |
| `rased_logo_dark.svg`, `.png` | The same for navy or blue backgrounds |
| `rased_logo_horizontal.svg`, `.png` | Mark beside RASED: **document headers** and slide footers |
| `rased_icon.svg`, `.png` | Mark only |
| `rased_appicon.svg`, `.png` | Mark on a navy rounded tile: avatar, favicon (readable down to 16 px) |
| `rased_banner.png` | 2560×1440 scene: container ship at night under a radar sweep, logo top-right, tagline "SEE YOUR SHIP COMING". **Jira board background**, slide backgrounds |
| `rased_readme_banner.png` | Wide crop of the banner (1600×739) for the top of the GitHub README |
| `rased_logo.py` | Regenerates the SVGs: `python brand/rased_logo.py`. PNGs are rendered from the SVGs at 4× (PyMuPDF) |

The v2 logo was rebuilt as a vector from the approved design sheet (`Chosen_navy_Outfit_capitals`), so it stays sharp at any print size.

## Don'ts

- Don't stretch the logo or recolour the sweep in anything other than signal blue (or translucent white on dark).
- Don't put the navy version on dark photos; use the dark version.
- Keep clear space around the logo of at least the radius of the mark on all sides.

## History

v1 (7–9 Oct 2026) was a lowercase "rasid" wordmark with a container ship as the dot of the "i", plus circular instrument emblems built from it. Its files were deleted on 14 Oct 2026; the committed ones remain in git history.

v2 RASID files were replaced by the RASED files on 14 Nov 2026 (the supervisor asked for the rename: “Rasid” reads as رصيد, phone credit). Same geometry, colours and font; only the wordmark changed. The old files remain in git history.
