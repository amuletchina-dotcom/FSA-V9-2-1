ASTM E1300 NFL V5 - GitHub Pages package

Upload all files and the charts folder to the repository root.

Features:
- Outer/Inner selection
- Monolithic or PVB laminated
- ASTM chart auto-selection
- Calibrated 4-side NFL calculation
- Original ASTM chart display
- Highlighted panel point, AR line, calibrated NFL contour and value

Calibration anchors:
- 6 mm monolithic 1200x1500 = 2.50 kPa
- 6 mm monolithic 1520x1900 = 1.80 kPa
- 6 mm monolithic 2000x2800 = 1.00 kPa
- 8 mm laminated 1520x1900 = 2.50 kPa

V6 FIXED:
- Fixed JavaScript syntax failure that prevented Calculate NFL from running.
- Fixed missing support-change event, so the Left/Right vs Top/Bottom selector now appears when 2-side support is selected.
- 2-side support passes supportedEdges into the NFL engine.
- A1.27/A1.43 output NFL plus highlighted thickness line, guides, intersection and value.

V6.1 RANGE FIX:
- A1.27 / A1.43 no longer report extrapolated precise NFL values outside the printed chart.
- Below chart minimum: display '< 0.25 kPa' for A1.27 or '< 0.20 kPa' for A1.43.
- Above chart maximum: display '> 10.00 kPa' or '> 8.00 kPa'.
- Monolithic 5 mm on A1.27 now displays 'NOT COVERED' instead of MANUAL.
- Out-of-range thickness line remains highlighted for visual review, but no fake intersection is plotted.

V6.2 ONE-EDGE:
- Added automatic ASTM one-edge/cantilever NFL for Fig. A1.28 (monolithic) and A1.44 (PVB laminated).
- When 1-side support is selected, user chooses Left / Right / Top / Bottom supported edge.
- Program derives the unsupported/cantilever length from panel width or height.
- A1.28 and A1.44 digitized line families use q = C/L^2, calibrated directly from the supplied ASTM chart images.
- Chart overlay highlights selected thickness/designation line, unsupported-length guide, intersection and NFL.
- Out-of-chart load or unsupported-length ranges are explicitly flagged instead of extrapolated as ASTM values.

V6.3 LAMINATED DESIGNATION DETAIL:
- Laminated input now shows ASTM minimum-thickness conversion for each glass ply.
- Example: 8 mm nominal -> 7.42 mm minimum; 10 mm nominal -> 9.02 mm minimum.
- Displays PVB contribution and total thickness used to select ASTM laminated designation.
- ASTM special designation cases are flagged when applied.

V7 GTF + LSF:
- Based directly on V6.3 source backup.
- Adds Single / Double IGU workflow with AN / HS / FT per lite.
- Adds Short Duration (<=3 s) / Long Duration (~30 days).
- GTF automatic lookup: ASTM Table 1 for single glazing; Tables 2/3 for Double IGU.
- GTF evidence table is rendered in-app with the selected ASTM cell highlighted.
- LSF automatic calculation follows ASTM Appendix X3 stiffness basis and identifies Table 5 / Table 6 applicability.
- LSF evidence is rendered in-app with the selected lite/result highlighted.
- NFL chart viewer and all 44 A1 figure files remain from V6.3.
- Single lite displays LSF = N/A.
- Double IGU with support other than four sides is explicitly flagged as outside the ASTM basic IGU procedure.

V7.1 LAMINATED PLY HEAT-TREATMENT TYPES:
- Each laminated ply now has its own AN / HS / FT selector.
- Laminated construction display preserves both ply thicknesses and both heat-treatment states.
- When both plies have the same type (AN+AN, HS+HS, FT+FT), automatic GTF lookup continues using ASTM Tables 1–3.
- When a laminated lite has mixed ply types (e.g. AN + PVB + HS), ASTM E1300-16 supplied source does not explicitly define which AN/HS/FT GTF category applies to that mixed lite.
- Mixed-ply GTF therefore shows REVIEW and no value is guessed.
- NFL designation and LSF calculation continue to work because their thickness/stiffness logic is independent of the heat-treatment label.

V7.2 ASTM GTF CLASSIFICATION:
- Laminated lites now include an "ASTM GTF Classification" selector: Auto / AN / HS / FT.
- Auto:
  * same ply heat treatment (AN+AN, HS+HS, FT+FT) -> automatic classification and GTF lookup.
  * mixed ply heat treatment (e.g. HS+AN) -> REVIEW because the supplied ASTM E1300-16 tables do not explicitly assign a category.
- Manual AN / HS / FT selection:
  * uses the user-selected category for ASTM Table 1 / 2 / 3 GTF lookup.
  * result and evidence panel explicitly mark the classification as user selected.
- NFL and LSF logic remains unchanged.

V7.3 AUTO WEAKEST-PLY GTF RULE:
- Laminated ply heat treatments remain independently selectable.
- Auto GTF classification now always resolves:
  * AN + HS -> AN
  * AN + FT -> AN
  * HS + FT -> HS
  * identical ply types -> that same type.
- This is a conservative PROJECT RULE ("weakest ply governs"), not an explicit ASTM E1300-16 mixed-ply classification rule.
- User can still override Auto and manually select AN / HS / FT.
- Result and evidence panels explicitly identify auto weakest-ply classifications versus user-selected classifications.
- NFL and LSF logic unchanged.

V8.0 DPT WIND INTEGRATION:
- Base: user's backed-up V7-4 ASTM E1300 package.
- Added Thailand DPT มยผ.1311-50 wind-load pre-check.
- Digitized Appendix A Table A-1 province wind groups for the 76 provinces in the 2007 standard.
- Special district logic included for Tak, Nakhon Si Thammarat and Surat Thani.
- Wind groups:
  Group 1 V50=25 m/s TF=1.00
  Group 2 V50=27 m/s TF=1.00
  Group 3 V50=29 m/s TF=1.00
  Group 4A V50=25 m/s TF=1.20
  Group 4B V50=25 m/s TF=1.08
- Chapter 2 simple-method reference pressure q = 0.5*rho*V^2 with rho=1.25 kg/m3.
- Simple-method terrain/exposure Ce formulas for A/B included.
- Importance factors digitized from Table 2-2.
- Small cladding/window Cg=2.5 and default internal Cgi=2.0 included.
- Internal pressure cases 1/2/3 digitized from Section 2.6.2.
- Windward Cp +0.8 and leeward Cp -0.5 presets included; custom Cp supported.
- Suggested pressure is a preliminary glass/cladding pressure and can be manually overridden.
- The final "Design Wind Pressure Used" is retained as the project input for future deflection / pass-fail integration.
- Local low-rise edge/corner coefficients still require the appropriate DPT Appendix B coefficient; use Custom Cp/manual override where applicable.

V8.1 DEFLECTION:
- Added ASTM E1300 deflection module connected to "Design Wind Pressure Used".
- Four-side monolithic glass:
  * automatic Appendix X1 approximate center deflection
  * uses ASTM Table 4 minimum thickness
  * E = 71.7 x 10^6 kPa
  * aspect ratio >5 is capped at 5 per X1
  * detailed formula evidence is shown in the Deflection tab
- Double IGU:
  * current BASIC estimate apportions design pressure to each lite using the current LSF before X1 deflection.
  * exact ASTM analytical IG deflection requires ideal-gas-law equilibrium / displaced-volume load sharing per 6.3.3.1; V8.1 does not claim that LSF estimate is the full analytical method.
- Laminated:
  * numerical automatic deflection is intentionally left as REVIEW because ASTM analytical deflection requires effective thickness hef,w from Appendix X9 using interlayer shear modulus for the temperature/load-duration condition.
  * original corresponding A1 lower deflection chart remains the ASTM basic-method source.
- 3-side / 2-side / 1-side:
  * shown as REVIEW until lower A1 deflection charts are numerically digitized; no unsupported X1 extrapolation is made.
- ASTM E1300 does not itself impose a universal aesthetic deflection limit; it requires deflection to be reviewed.

V8.2 FINAL PASS / FAIL:
- Added final engineering-result summary: PASS / FAIL / REVIEW.
- Strength check:
  * PASS when ASTM Load Resistance (LR) >= Design Wind Pressure Used.
  * FAIL when LR < Design Wind Pressure Used.
  * REVIEW when LR is unavailable for automatic judgement.
- Deflection criterion options:
  * Thailand Typical — L/90 and maximum 20 mm (PROJECT DEFAULT)
  * L/60
  * L/100
  * ASTM E1300 — Review only
  * Custom mm
- Thailand Typical logic: allowable deflection = min(short glass dimension / 90, 20 mm).
- ASTM E1300 itself does NOT impose this Thailand project limit; it requires glass deflection to be reviewed.
- Final result:
  * FAIL if any lite has Strength FAIL or Deflection FAIL.
  * REVIEW if there is no FAIL but any required check remains REVIEW.
  * PASS only when all automatic strength and deflection checks pass.

V8.3 EDGE-SUPPORT DEFLECTION:
- Added automatic deflection for:
  * 3-side support: ASTM Annex A1 lower deflection charts A1.15-A1.26 (monolithic) and A1.36-A1.42 (laminated)
  * 2-opposite-side support: lower charts A1.27 / A1.43
  * 1-edge cantilever: lower charts A1.28 / A1.44
- Three-side input now asks which edge is the free/unsupported edge.
- Deflection locations follow ASTM:
  * 3-side: center of unsupported edge
  * 2-side: center of an unsupported edge
  * 1-side: free edge opposite the supported edge
- The new numerical values use calibrated digitized interpolation of the ASTM lower charts and preserve the printed chart range.
- Points outside the digitized chart range return REVIEW instead of extrapolating.
- 4-side monolithic still uses ASTM Appendix X1.
- 4-side laminated remains REVIEW because the analytical route requires Appendix X9 effective thickness / interlayer shear modulus.
- Strength for 3-side remains REVIEW in this build because the 3-side NFL upper charts have not yet been fully numerically digitized.

V8.4 FOUR-SIDE LAMINATED DEFLECTION
- Fixes the previous REVIEW-only behavior for four-side PVB laminated glass.
- ASTM basic procedure source remains the lower deflection charts A1.29-A1.35.
- Automatic numerical deflection now uses ASTM Appendix X1 as the approximate
  four-side deflection procedure, with the laminated ASTM designation mapped
  to its Table 4 minimum thickness.
- Example: 6 + 1.52 PVB + 6 -> ASTM designation 12 -> minimum thickness 11.91 mm
  for the automatic four-side deflection calculation.
- The existing Thailand Typical pass/fail criterion remains:
  allowable = min(short glass dimension / 90, 20 mm).

V8.5 EMPLOYEE LOGIN + FULL REPORT
- Employee selection/login added: Benz, Mook, Dear, Nee, BB.
- Employee choice is stored locally in the browser and printed in reports.
- This is identification only, not secure server authentication, because GitHub Pages is static.
- Added optional report metadata: Project Name, Client, Reference / Drawing No.
- Added Generate Full Report after calculation.
- Report opens in a new page and includes:
  DPT wind calculation and wind-pressure chart;
  project/panel/input summary;
  final PASS/FAIL/REVIEW;
  ASTM NFL/GTF/LSF/LR and strength checks for each lite;
  deflection calculation, criterion and PASS/FAIL;
  load resistance vs design pressure chart;
  deflection vs allowable chart;
  corresponding ASTM Annex A1 source figure(s);
  basis / limitations / notes;
  employee and timestamp.
- Report page supports Print / Save PDF and Download HTML.

V8.6 REPORT / INPUT UPDATE
- Added report metadata: Panel Code, Floor, Coating Code.
- Added Air Gap selector for IGU: 6 / 9 / 12 / 16 / 20 / 24 mm.
- Added live Overall Glass Structure preview and placed the complete structure prominently at the top of the generated report.
- Glass thickness selectors changed to project list: 4 / 5 / 6 / 8 / 10 / 12 / 15 / 19 mm.
- IMPORTANT: ASTM E1300-16 Table 4 / Annex A1 does not provide a 15 mm monolithic nominal designation. 15 mm remains selectable as a project thickness but monolithic ASTM NFL returns REVIEW rather than silently mapping it to 16 mm.
- Report ASTM source figures now carry a visible highlighted NFL result badge.
- Report page now has a Return to Calculator button.

V8.7 COATING LAYERS / SURFACES
- Added Coating Layer(s): 1 or 2.
- Each coating has its own Coating Code and Surface selector (#2 / #3 / #4 / #5 / #6).
- Overall Glass Structure preview now places the coating code on the glass ply that owns the selected surface.
- Example:
  Coating 1 RTS01 on #2 and Coating 2 RTM2M2 on #4
  6 mm FT (RTS01)#2 + 1.52 mm PVB + 6 mm FT (RTM2M2)#4 + 12 mm Air Gap + 8 mm FT
- Generated report shows:
  coating layer count;
  each coating code + surface;
  complete overall glass structure with coating code/surface embedded.

V8.8 REPORT FILE NAME
- Default report filename format changed to:
  CS ECO_Project Name_Panel Code_DDMMYYYY_HHMM
- Example:
  CS ECO_Pan Pacific_A01_27092026_1608
- The same name is used as the report document title and the downloaded HTML filename.
- Invalid filename characters are automatically replaced.

V8.9
- Added Assembly: Laminated Only. It hides Air Gap and forces the visible lite to Laminated.
- Every monolithic glass and every laminated ply can select:
  Clear / Green Tinted / Grey Tinted / Low Iron.
- Laminated interlayer material can be PVB or SGP.
- PVB type:
  Clear PVB / Color PVB / Acoustic PVB / 100% UV Transmittance PVB / Frosted PVB.
- Interlayer thickness:
  0.38 / 0.76 / 1.14 / 1.52 / 2.28 / 2.66 / 3.04 mm.
- Overall Structure and report now include glass appearance, coating code/surface, and interlayer type/thickness.
- Print report layout was tightened so the text that previously spilled to page 6 is kept under the page-5 section where possible.

V9.0 MIXED LAMINATED PLY GTF
- For laminated glass with the SAME heat treatment on both plies:
  GTF classification is automatic from that common type.
- For laminated glass with DIFFERENT ply heat treatments (for example 4 AN + PVB + 6 HS):
  default ASTM Basic Procedure result is GTF = REVIEW.
- Reason:
  ASTM E1300 Table 1 provides AN / HS / FT GTF categories for a laminated lite,
  but does not explicitly assign a Basic Procedure category for mixed heat-treatment plies.
- User can explicitly select:
  * Project conservative — weakest ply
  * User selected — AN / HS / FT
- When Project conservative — weakest ply is selected:
  the weakest heat-treatment category is used for Table 1 GTF and the report labels this
  as a project conservative rule, not an explicit ASTM mixed-ply rule.
- The full design wind pressure still applies to the laminated lite as one lite.
  The two plies are not assigned separate shares of wind pressure in the Basic Procedure.

V9.1 DUAL INTERLAYER / NO PVB-SGP MIXING
- Laminated glass always shows two interlayer layers.
- A single Interlayer Material selector applies to BOTH layers:
  PVB or SGP.
- PVB and SGP cannot be mixed in one laminate.
- PVB Layer 1 and Layer 2 each independently select:
  Clear PVB / Color PVB / Acoustic PVB / 100% UV Transmittance PVB / Frosted PVB.
- Each interlayer layer independently selects thickness:
  0.38 / 0.76 / 1.14 / 1.52 / 2.28 / 2.66 / 3.04 mm.
- If SGP is selected, both layers are fixed to SGP while retaining independent thickness selection.
- ASTM laminated designation uses the sum of both interlayer layer thicknesses, capped at 1.52 mm for designation per ASTM E1300.
- Surface Stress has been added as a visible REVIEW item for laminated glass.
  No numerical stress is generated without the ASTM Analytical Procedure inputs, including interlayer shear modulus G.

V9.2 ASTM X9 SHEAR MODULUS / EFFECTIVE THICKNESS
- Added Interlayer Shear Modulus G for laminated glass.
- Default wind-load values at 50°C / 3 s:
  * PVB: G = 0.40 MPa (ASTM E1300 Appendix X8 reference value)
  * SGP / SentryGlas Xtra: G = 12.7 MPa
- Added Custom G mode for manufacturer-specific values.
- PVB and SGP remain prohibited from mixing in one laminate.
- Added ASTM Appendix X9 calculations:
  * shear transfer coefficient Gamma
  * effective thickness for deflection hef,w
  * effective stress thickness h1,ef,sigma
  * effective stress thickness h2,ef,sigma
- Added Appendix X6 conservative 3-second allowable surface-stress references:
  * AN 23.3 MPa
  * HS 46.6 MPa
  * FT 93.1 MPa
- Actual maximum surface tensile stress remains REVIEW.
  X9 provides effective thicknesses; ASTM 6.3.2 still requires A2.1 /
  rigorous nonlinear plate-stress analysis for the actual stress.

V9.2.1 CALCULATE HOTFIX
- Fixed Calculate-button runtime error introduced in V9.2.
- Cause: app.js result rendering called fmt(), while the existing fmt helper was private to report-engine.js.
- Added a local app.js fmt() helper for numeric result rendering.
