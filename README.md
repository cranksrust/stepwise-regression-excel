# Stepwise Regression for Excel

An Office web add-in that runs forward selection, backward elimination and stepwise regression in Excel for Windows, Mac and the web. It writes a report in the layout of Excel's Data Analysis > Regression tool, with Standard Error shown as RMSE. It uses no VBA, so students never touch macro security or the Trust Center.

**Install page:** https://cranksrust.github.io/stepwise-regression-excel/

## Install (students)

The add-in is not in the Office Add-ins store yet, so it is installed from a small file called `manifest.xml`. This takes about two minutes and only needs to be done once per browser or computer.

### Step 1: Download the manifest

1. Go to https://cranksrust.github.io/stepwise-regression-excel/
2. Click **Download manifest.xml** and save it somewhere easy to find, such as Downloads.

### Step 2: Add it to Excel

**Excel for the web (recommended, works for everyone)**

1. Go to [office.com](https://www.office.com/launch/excel) and open any workbook.
2. Open **Add-ins**. Where it is depends on your window width:
   - Wide window: **Home** tab, **Add-ins** button at the right end of the ribbon.
   - Narrow window: click the **⋯** at the right end of the Home ribbon, or use the **Insert** tab, **Add-ins**.
3. Click **More Add-ins** (on some versions, **Advanced**).
4. Open the **My Add-ins** tab and click **Upload My Add-in** near the top right.
5. Click **Browse**, choose `manifest.xml`, and click **Upload**.
6. A **Stepwise Regression** button appears on the Home tab (in a narrow window, it may be under the **⋯**).

**Excel for Mac**

1. Quit Excel.
2. In Finder, press **Cmd+Shift+G**, paste `~/Library/Containers/com.microsoft.Excel/Data/Documents/wef` and press Return. If it says the folder does not exist, go to `~/Library/Containers/com.microsoft.Excel/Data/Documents` and create a folder named `wef`.
3. Copy `manifest.xml` into the `wef` folder.
4. Open Excel and a workbook, then go to **Home > Add-ins** (or **Insert > Add-ins > My Add-ins**). **Stepwise Regression** is listed under the developer or shared add-ins section. Click it once and the button appears on the Home tab.

**Excel for Windows (desktop)**

Go to **Home > Add-ins > More Add-ins > My Add-ins**. If **Upload My Add-in** is shown, follow steps 4 to 6 above. If it is not, use Excel for the web.

### Step 3: Run a regression

1. Click **Stepwise Regression** on the Home tab. A panel opens on the right.
2. Select your Y column, including its label cell, and click **Use selection** next to **Y range**.
3. Select your X columns, including their label cells, and click **Use selection** next to **X range**. Hold Ctrl (Cmd on Mac) to pick columns that are not next to each other.
4. Leave **First row contains labels** ticked if you included the labels.
5. Choose the **Method** (Stepwise is the default) and **Criterion** (T-Stat, entering and removing at |T-Stat| = 2.00, is the default).
6. Click **Run regression**. The report opens on a new sheet named **Stepwise**.

### If something goes wrong

| Problem | Fix |
|---|---|
| No **Add-ins** button | The window is too narrow. Check under **⋯** on the Home ribbon or the **Insert** tab, or widen the window. |
| **Upload My Add-in** is missing or greyed out | Your school or work account may block custom add-ins. Sign in to office.com with a personal Microsoft account and try again. |
| The button disappeared | In Excel for the web, uploaded add-ins are remembered per browser. Upload the manifest again. |
| "Every X column must start and end on the same rows as Y" | Select the same rows for Y and X, for example `A4:A48` and `B4:F48`. |
| "The model with every predictor cannot be estimated" | Two X columns carry the same information (for example one is a fixed number minus the other). Remove one, or use Forward or Stepwise. |

More help: https://cranksrust.github.io/stepwise-regression-excel/support.html

## How it works

The add-in is a small static website. Excel loads `docs/taskpane.html` in a side panel, and everything runs in the browser inside Excel: no server, no data leaves the workbook. GitHub Pages hosts it for free.

| Path | Purpose |
|---|---|
| `docs/` | The published site: task pane, engine, report writer, icons, install/help/privacy pages |
| `docs/stepwise.js` | Regression engine and t/F distributions, no Office dependency |
| `docs/report.js` | Builds the report layout and writes it with the Excel JavaScript API |
| `docs/taskpane.*` | Side panel UI |
| `manifest.template.xml` | Add-in manifest with a `{{BASE_URL}}` placeholder |
| `build-manifest.js` | Writes `docs/manifest.xml` for the site's address |
| `test/` | Engine, report and browser end-to-end tests |

## One-time setup

1. **Repository and Pages.** Push this folder to the root of a public GitHub repository. In the repository, go to **Settings > Pages**, set **Source** to **Deploy from a branch**, branch **main**, folder **/docs**, and save. The site appears at `https://<github-user>.github.io/<repo>/` within a minute or two.
2. **Manifest.** Run `node build-manifest.js https://<github-user>.github.io/<repo>`, then commit and push `docs/manifest.xml`.
3. **Validate.** Run `npx office-addin-manifest validate docs/manifest.xml`. It should report the manifest as valid.
4. **Try it.** Open `https://<github-user>.github.io/<repo>/`, download `manifest.xml`, and follow the Excel for the web steps on that page.

## Publishing to the Office Add-ins store (AppSource)

1. **Publisher account.** Create a Microsoft Partner Center account and enroll in the Microsoft 365 and Copilot program. Use an account the program will keep, since the listing lives there. Verification can take several days.
2. **New offer.** In Partner Center, create a new **Office add-in** offer named **Stepwise Regression** and upload `docs/manifest.xml`.
3. **Listing details:**
   - Short and long description (start from the manifest description and the install page).
   - Logo: `docs/assets/icon-300.png`.
   - Screenshots at 1366x768: the task pane next to a finished report works well.
   - Privacy policy URL: `https://<github-user>.github.io/<repo>/privacy.html`
   - Support URL: `https://<github-user>.github.io/<repo>/support.html`
   - Category: Data Analytics.
4. **Notes for certification.** Tell the testers it needs no sign-in, and give them a short data set and steps, for example: "Put numbers in A1:D20 with headers in row 1, open Stepwise Regression from the Home tab, use A1:A20 as Y and B1:D20 as X, click Run regression."
5. **Submit.** Review usually takes several business days. Fix anything the report flags and resubmit.

Code changes in `docs/` go live as soon as they are pushed, with no store review. Changes to the manifest (name, icons, buttons, permissions, version) need a new submission, and the `<Version>` number must go up.

## Development

```
npm install            # only needed for the browser end-to-end test
npm test               # engine and report tests (Node 18+)
npm run test:e2e       # task pane in headless Chromium against a mock workbook
```

`test/make_fixtures.py` regenerates `test/fixtures.json` from the Python reference implementation in the VBA project (`excel-stepwise-regression/tests`) and scipy.

## Handoff

The add-in keeps working only while the GitHub repository and Pages site stay up and the Partner Center account stays active. Before the maintainer leaves, transfer the repository (**Settings > Transfer ownership**) and the Partner Center offer to the next owner.
