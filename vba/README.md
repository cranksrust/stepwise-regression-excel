# Stepwise Regression: VBA version (backup)

The original desktop-only version of the add-in, kept as a backup. **Most people should use the web add-in in the [main README](../README.md)**, which needs no macros or Trust Center changes and also works in Excel for the web.

This version is a VBA add-in (`.xlam`) for desktop Excel on Windows and Mac. It produces the same report and adds a `=STEPREG()` worksheet function.

![Example report](../docs/assets/report-preview.png)

## Install

### Option A: build script (Windows)

1. In Excel, turn on **File > Options > Trust Center > Trust Center Settings > Macro Settings > Trust access to the VBA project object model**.
2. Download this folder and run `powershell -ExecutionPolicy Bypass -File .\Build-Addin.ps1` in it. It writes `StepwiseRegression.xlam`.

### Option B: by hand (Windows or Mac)

1. Download `StepwiseRegression.bas` (on GitHub, open the file and click the download button).
2. Open a blank workbook and press **Alt+F11** (Mac: **Tools > Macro > Visual Basic Editor**).
3. **File > Import File...** and pick `StepwiseRegression.bas`.
4. Double-click **ThisWorkbook** under the workbook's project and paste:
   ```vb
   Private Sub Workbook_Open()
       Auto_Open
   End Sub

   Private Sub Workbook_BeforeClose(Cancel As Boolean)
       Auto_Close
   End Sub
   ```
5. **Debug > Compile VBAProject**, then close the editor.
6. **File > Save As**, choose **Excel Add-in (\*.xlam)**. Excel switches to its AddIns folder.

### Load it

1. **File > Options > Add-ins**, set **Manage** to **Excel Add-ins**, click **Go...**, tick **StepwiseRegression** (use **Browse** if it is not listed). On Mac: **Tools > Excel Add-ins**.
2. If Excel blocks the macros, add the AddIns folder under **Trust Center > Trusted Locations**. If the file was downloaded, right-click it, choose **Properties**, and tick **Unblock**.
3. Add a Ribbon button: **File > Options > Customize Ribbon**, select **Data** on the right, **New Group**, then set **Choose commands from** to **Macros**, select **RunStepwiseRegression** and click **Add >>**.

## Use

Click the Ribbon button (or right-click a cell and choose **Stepwise Regression...**). The prompts ask for the Y range, the X range, whether the first row has labels, the method (1 Forward, 2 Backward, 3 Stepwise), the criterion (1 T-Stat, 2 P-value) and the thresholds. The report opens on a new sheet named **Stepwise**.

### Worksheet function

```
=STEPREG(known_y, known_x, [method], [criterion], [enter_threshold], [remove_threshold], [has_labels])
```

Returns the final coefficient table. Defaults: method 3 (stepwise), criterion 1 (T-Stat), thresholds 2 and 2 (or 0.05 and 0.10 for P-value). Example with the [sample data](../docs/sample/home-prices.xlsx): `=STEPREG(A1:A61, B1:I61, 3, 1, 2, 2, TRUE)`. It spills in Excel 365. In older versions, select a block 5 columns wide, type the formula and press **Ctrl+Shift+Enter**.

## Method

The selection rules, report layout and numbers are identical to the web add-in. The VBA code and the web add-in's JavaScript are both checked against the Python reference in [`test/reference/`](../test/reference/).
