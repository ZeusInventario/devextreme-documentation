---
EventForAction: ..\4 Events\editorPreparing.md
default: null
type: function(e)
---
---
##### shortDescription
A function that is executed before an editor is created.

##### param(e): Object
Information about the event.

##### field(e.component): {WidgetName}
The widget's [instance](/api-reference/10%20UI%20Widgets/Component/3%20Methods/instance().md '/Documentation/ApiReference/UI_Widgets/dxFilterBuilder/Methods/#instance').

##### field(e.element): dxElement
The widget's container. It is an [HTML Element](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) or a [jQuery Element](https://api.jquery.com/Types/#jQuery) when you use jQuery.

##### field(e.model): Object
The model data. Available only if you use Knockout.

##### field(e.value): any
The editor's value.

##### field(e.setValue(newValue)): any
A method that you should call to change the field's value after the editor's value changes.

##### field(e.cancel): Boolean
Allows you to cancel the creation of the editor.    
Set it to **true** and implement a custom editor if your scenario requires it.

##### field(e.editorElement): dxElement
The editor's container. It is an [HTML Element](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) or a [jQuery Element](https://api.jquery.com/Types/#jQuery) when you use jQuery.

##### field(e.editorName): String
Allows you to change the editor. Accepts names of DevExtreme widgets only, for example, *"dxTextBox"*.      
Import a new editor's module when using [DevExtreme modules](/concepts/Common/30%20Modularity '/Documentation/Guide/Common/Modularity/').

##### field(e.editorOptions): Object
Gets and sets the editor configuration.

##### field(e.dataField): String
The data field's name.

##### field(e.filterOperation): String
The applied filter operation.

##### field(e.updateValueTimeout): Number
Gets and sets the delay between when a user stops typing the field value and when it is applied.

##### field(e.width): Number
The editor's width.

##### field(e.readOnly): Boolean
Indicates whether the editor is read-only.

##### field(e.disabled): Boolean
Indicates whether the editor is disabled.

##### field(e.rtlEnabled): Boolean
Indicates whether the editor uses right-to-left representation.

---
The widget offers a user different editors for entering a value depending on the field's [dataType](/api-reference/10%20UI%20Widgets/dxFilterBuilder/5%20Field/dataType.md '/Documentation/ApiReference/UI_Widgets/dxFilterBuilder/Field/#dataType'): [Calendar](/concepts/05%20Widgets/Calendar/00%20Overview.md '/Documentation/Guide/Widgets/Calendar/Overview/'), [TextBox](/concepts/05%20Widgets/TextBox/00%20Overview.md '/Documentation/Guide/Widgets/TextBox/Overview/'), [SelectBox](/concepts/05%20Widgets/SelectBox/00%20Overview.md '/Documentation/Guide/Widgets/SelectBox/Overview/'), and so on. Use this function to customize those default editors or substitute them for other editors. 

In the following code, a default editor is replaced with the DevExtreme [TextArea](/concepts/05%20Widgets/TextArea/00%20Overview.md '/Documentation/Guide/Widgets/TextArea/Overview/') widget. Note that the widget's **onValueChanged** function is overridden, and its declaration ends with the **setValue(newValue, newText)** method's call. This method updates the value.

---
##### jQuery

    <!--JavaScript-->
    $(function() {
        $("#filterBuilder").dxFilterBuilder({
            // ...
            onEditorPreparing: function (e) {
                if (e.dataField == "description") {
                    e.editorName = "dxTextArea"; 
                    e.editorOptions.showClearButton = true;
                    e.editorOptions.onValueChanged = function(event) {
                        var value = event.value;
                        e.setValue(value.toLowerCase()); 
                    }
                }
            }
        });
    });

##### Angular

    <!--TypeScript-->
    import { DxFilterBuilderModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        onEditorPreparing (e) { 
            if (e.dataField == "description") {
                e.editorName = "dxTextArea"; 
                e.editorOptions.showClearButton = true;
                e.editorOptions.onValueChanged = (event) => {
                    let value = event.value;
                    e.setValue(value.toLowerCase()); 
                }
            }
        }
    }
    @NgModule({
        imports: [
            // ...
            DxFilterBuilderModule
        ],
        // ...
    })

    <!--HTML-->
    <dx-filter-builder ...
        (onEditorPreparing)="onEditorPreparing($event)">
    </dx-filter-builder>
    
---

The following code shows how to replace a default editor with a non-DevExtreme editor (an HTML checkbox in this case):

---
##### jQuery

    <!--JavaScript-->
    $(function() {
        $("#filterBuilder").dxFilterBuilder({
            // ...
            onEditorPreparing: function(e) {
                if(e.dataField === "completed") {
                    e.cancel = true; // Cancels creating the default editor
                    $('<input type="checkbox">')
                        .prop("checked", e.value)
                        .on("change", function(event) {
                            e.setValue(event.target.checked);
                        })
                        .appendTo(e.editorElement);
                }
            }
        });
    });

##### Angular

    <!--TypeScript-->
    import { DxFilterBuilderModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        onEditorPreparing (e) { 
            if(e.dataField === "completed") {
                e.cancel = true; // Cancels creating the default editor
                let checkbox = document.createElement("INPUT");
                checkbox.setAttribute("type", "checkbox");
                checkbox.setAttribute("checked", e.value);
                checkbox.addEventListener("change", (event) => {
                    e.setValue(event.target.checked);
                });
                e.editorElement.appendChild(checkbox);
            }
        }
    }
    @NgModule({
        imports: [
            // ...
            DxFilterBuilderModule
        ],
        // ...
    })

    <!--HTML-->
    <dx-filter-builder ...
        (onEditorPreparing)="onEditorPreparing($event)">
    </dx-filter-builder>
    
---

[note]This function is not executed for fields that use the [editorTemplate](/api-reference/10%20UI%20Widgets/dxFilterBuilder/5%20Field/editorTemplate.md '/Documentation/ApiReference/UI_Widgets/dxFilterBuilder/Field/#editorTemplate').