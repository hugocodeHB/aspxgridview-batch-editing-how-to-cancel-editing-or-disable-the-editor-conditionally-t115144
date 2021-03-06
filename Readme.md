# ASPxGridView - Batch Editing - How to cancel editing or disable the editor conditionally


<p><strong>Starting from version 17.1,</strong> we introduced a new client-side <a href="https://documentation.devexpress.com/#AspNet/DevExpressWebScriptsASPxClientGridView_FocusedCellChangingtopic">FocusedCellChanging</a> event. This event allows you to skip focusing and editing a cell. Refer to the <a href="https://www.devexpress.com/Support/Center/p/T496531">ASPxGridView - Batch Edit mode - How to use the client-side FocusedCellChanging event and cancel editing or disable an editor conditionally</a> example for more details.<br><br><strong>For versions 16.2 and below:</strong><br>This example demonstrates how to cancel editing or disable the editor conditionally for the grid when batch editing is in use. It is possible to execute your logic either on the client or server side for a complex business model.<br>Then, handle the grid's client-side <a href="https://documentation.devexpress.com/#AspNet/DevExpressWebASPxGridViewScriptsASPxClientGridView_BatchEditStartEditingtopic">BatchEditStartEditing</a> event to either cancel the edit operation using the e.cancel property:</p>


```js
if (condition) e.cancel = true;

```


<p> or disable the editor by obtaining its client instance:</p>


```js
var editor = s.GetEditor(e.focusedColumn.fieldName);
editor.SetEnabled(condition);

```


<p> The custom server-side logic can be executed in the CustomJSProperties event handler:</p>


```cs
protected void ASPxGridView1_CustomJSProperties(object sender, DevExpress.Web.ASPxGridView.ASPxGridViewClientJSPropertiesEventArgs e) {
    var clientData = new Dictionary<int, object>();
    var grid = sender as ASPxGridView;
    for (int i = grid.VisibleStartIndex; i < grid.VisibleStartIndex + grid.SettingsPager.PageSize; i++) {
        var rowValues = grid.GetRowValues(i, new string[] { "ID", "ServerSideExample" }) as object[];

        var key = Convert.ToInt32(rowValues[0]);
        if (key % 2 != 0)
            clientData.Add(key, "ServerSideExample");
    }
    e.Properties["cp_cellsToDisable"] = clientData;
}
```


<p> </p>
<p><strong>See Also:<br><a href="https://www.devexpress.com/Support/Center/p/T101164">ASPxGridView - How to disable editing for rows that match some condition in Batch Edit Mode</a> <br><a href="https://www.devexpress.com/Support/Center/p/T115116">GridView - Batch Editing - How to cancel editing or disable the editor conditionally</a> </strong></p>

<br/>


