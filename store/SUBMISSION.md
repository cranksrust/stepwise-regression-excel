# Office Add-ins store (AppSource) submission

Everything needed to list **HOUMBA-28 Stepwise Regression** in the Office Add-ins store, in the order you will need it. Partner Center's screens change from time to time, so follow its prompts where they differ from the wording here.

Items marked **TODO** still need a decision.

## Part 1: Create a Partner Center account (one time)

1. Go to https://partner.microsoft.com and choose to join or sign up as a developer.
2. **Sign-in account.** Partner Center expects a work account (Microsoft Entra ID).
   - Do **not** use your UT account. The listing would then belong to the university's tenant.
   - If you only have a personal email, the sign-up flow offers to create a new work account for you (for example `you@yourname.onmicrosoft.com`). Keep its password safe, because it owns the listing.
3. **Program.** Enroll in the **Microsoft 365 and Copilot** program (the one that covers Office add-ins).
4. **Account type.** Choose **Individual** if offered. If only **Company** is available, stop and check before continuing, because company accounts ask for business verification documents.
5. **Publisher display name.** `HOUMBA Class of '28` (this matches the manifest's ProviderName).
6. **Contact details and verification.** Verify the email address Microsoft sends to. Identity verification can take several days. Partner Center shows its status on the account settings page.

There is no registration fee for Office add-ins as of this writing, but confirm on the sign-up screens.

## Part 2: Create the offer

In Partner Center, go to **Marketplace offers > Microsoft 365 and Copilot** (the exact menu name may differ), then **+ New offer > Office add-in**.

| Field | Value |
|---|---|
| Offer name | HOUMBA-28 Stepwise Regression |
| Manifest | Upload `docs/HOUMBA-28-Stepwise-Regression.xml` from this repo |
| Products | Excel |
| Platforms | Windows, Mac, Web (leave iPad unticked unless it has been tested there) |
| Category | Data Analytics (secondary: Education) |
| Industries | Education (optional) |
| Logo | `docs/assets/icon-300.png` (300 x 300) |
| Screenshots | `store/screenshot-1-panel.png`, `store/screenshot-2-report.png`, `store/screenshot-3-sample.png` (1366 x 768) |
| Privacy policy URL | https://cranksrust.github.io/stepwise-regression-excel/privacy.html |
| Support URL | https://cranksrust.github.io/stepwise-regression-excel/support.html |
| Help / documentation URL | https://cranksrust.github.io/stepwise-regression-excel/ |
| Support email | **TODO** (a monitored address; reviewers and users may write to it) |
| End-user license agreement | Standard contract, or link to `LICENSE` in the repo |
| Pricing | Free |
| Requires an account or sign-in | No |
| Requires additional purchase | No |

### Short description (up to 100 characters)

```
Forward, backward and stepwise regression with reports in Excel's Data Analysis Regression layout.
```

### Long description

```
HOUMBA-28 Stepwise Regression adds forward selection, backward elimination and stepwise regression to Excel.

Pick a Y column and the candidate X columns, choose a method, and click Run regression. The add-in writes a report to a new sheet in the same layout as Excel's Data Analysis > Regression tool, so it reads like the output you already know.

Features
- Three methods: forward selection, backward elimination, and stepwise (forward entry with backward removal).
- Two criteria: T-Stat (enter when |T-Stat| is at least 2.00, remove when it falls below 2.00) or p-value (enter below 0.05, remove above 0.10). Thresholds are adjustable.
- A step-by-step history of every variable entered or removed, with its T-Stat and p-value and the model's R Square, Adjusted R Square, RMSE, AIC and BIC after each step.
- The final model's Regression Statistics, ANOVA and coefficient tables with 95% confidence intervals.
- The T-Stat and p-value each excluded variable would have if it were added next.
- Handles labels, skips rows with blanks or text, and flags constant or perfectly collinear columns.
- Works in Excel for the web, Windows and Mac. No macros and no sign-in.

Privacy
All calculations run inside Excel. No workbook data leaves your workbook, and the add-in uses no cookies, analytics or tracking.

Built by the HOUMBA Class of '28 for MBA statistics coursework. A free sample workbook is available on the help page.
```

### Search keywords (up to 3)

```
stepwise regression, multiple regression, statistics
```

## Part 3: Notes for certification (testing instructions field)

```
No sign-in or account is required.

Test data: download the sample workbook from
https://cranksrust.github.io/stepwise-regression-excel/sample/home-prices.xlsx
and open it in Excel.

Steps:
1. Open the "Home Prices" sheet.
2. On the Home tab, click "Stepwise Regression" to open the task pane.
3. Select A1:A61 and click "Use selection" next to Y range.
4. Select B1:I61 and click "Use selection" next to X range.
5. Leave the defaults (First row contains labels, Stepwise, T-Stat, 2 and 2) and click "Run regression".

Expected result: a new sheet named "Stepwise" with a report. Under "Selection steps", TAX_ASSESSED enters at step 1 and is removed at step 5. The final model has SQFT, AGE, MILES_DOWNTOWN and SCHOOL_RATING with R Square 0.9070.

Error handling can be checked by setting X range to B2:I61 (different rows from Y). The pane shows "Every X column must start and end on the same rows as Y".
```

## Part 4: Submit and follow up

1. Review every page in the offer, then click **Review and publish**, then **Publish**.
2. Microsoft's certification usually takes several business days. Partner Center emails the result.
3. If the report lists issues, fix them in this repo, push (code changes go live right away), and resubmit. If the fix changes the manifest, increase `<Version>` in `manifest.template.xml`, run `node build-manifest.js https://cranksrust.github.io/stepwise-regression-excel`, and upload the new `docs/HOUMBA-28-Stepwise-Regression.xml` with the resubmission.
4. Once it is live, update the README and install page so that installing from the store is step 1.

## Checklist

- [ ] Partner Center account created with a work account you will keep (not UT)
- [ ] Publisher verified
- [ ] Support email chosen and monitored (**TODO**)
- [ ] Offer created, manifest uploaded
- [ ] Listing text, logo and three screenshots added
- [ ] Privacy, support and help URLs added and loading
- [ ] Testing notes pasted
- [ ] Submitted
