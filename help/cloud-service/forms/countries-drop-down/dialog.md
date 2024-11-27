---
title: 建立國家/地區元件的對話方塊
description: 建立元件的對話方塊
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 1%

---

# 建立國家元件的對話方塊

國家/地區元件繼承下拉式元件的對話方塊結構，但引入稱為大陸的新屬性。 此外，對話方塊經過自訂，可隱藏從下拉式元件繼承的特定欄位，同時允許作者選取所需的洲。

建立此對話方塊的最簡單方法如下：

1. 在您的AEM專案中，在countries元件資料夾底下建立一個名為_cq_dialog的資料夾。
2. 在_cq_dialog資料夾內，建立名為.content.xml的檔案。
3. 將下方提供的XML程式碼貼入此檔案中。
4. 儲存您的變更，並與AEM同步專案。

這會新增國家/地區元件的對話方塊設定。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:primaryType="nt:unstructured"
    jcr:title="Country of Residence"
    sling:resourceType="cq/gui/components/authoring/dialog">
    <content
        jcr:primaryType="nt:unstructured"
        sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                jcr:primaryType="nt:unstructured"
                sling:resourceType="granite/ui/components/coral/foundation/tabs"
                maximized="true">
                <items jcr:primaryType="nt:unstructured">
                    <basic
                        jcr:primaryType="nt:unstructured"
                        jcr:title="Basic"
                        sling:resourceType="granite/ui/components/coral/foundation/container">
                        <items jcr:primaryType="nt:unstructured">
                            <columns
                                jcr:primaryType="nt:unstructured"
                                sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                margin="{Boolean}true">
                                <items jcr:primaryType="nt:unstructured">
                                    <column
                                        jcr:primaryType="nt:unstructured"
                                        sling:resourceType="granite/ui/components/coral/foundation/container">
                                        <items jcr:primaryType="nt:unstructured">
                                            <saveValueType
                                                granite:class="cmp-adaptiveform-dropdown__savevaluetype"
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="placeholder"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/select"
                                                fieldLabel="Save value as"
                                                name="./typeIndex"
                                                typeHint="String">
                                                <items jcr:primaryType="nt:unstructured">
                                                    <String
                                                        jcr:primaryType="nt:unstructured"
                                                        text="String"
                                                        value="0"/>
                                                    <Number
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Number"
                                                        value="1"/>
                                                    <Boolean
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Boolean"
                                                        value="2"/>
                                                </items>
                                            </saveValueType>
                                            <type
                                                jcr:primaryType="nt:unstructured"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
                                                name="./type"/>
                                            <enums
                                                jcr:primaryType="nt:unstructured"
                                                sling:hideResource="{Boolean}true"
                                                sling:orderBefore="bindref"
                                                sling:resourceType="granite/ui/components/coral/foundation/container">
                                                <items jcr:primaryType="nt:unstructured">
                                                    <enumOptions
                                                        jcr:primaryType="nt:unstructured"
                                                        sling:orderBefore="bindref"
                                                        sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                        fieldDescription="Provide the pair of enum (data value) and enumName (display text) for each option."
                                                        fieldLabel="Options">
                                                        <field
                                                            jcr:primaryType="nt:unstructured"
                                                            sling:resourceType="granite/ui/components/coral/foundation/container"
                                                            name="./enum">
                                                            <items jcr:primaryType="nt:unstructured">
                                                                <enum
                                                                    granite:class="cmp-adaptiveform-base__enum"
                                                                    jcr:primaryType="nt:unstructured"
                                                                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                                    fieldDescription="Specify the content to submit, when the option is selected."
                                                                    fieldLabel="Data value"
                                                                    name="./enum"
                                                                    required="{Boolean}true"/>
                                                                <enumNames
                                                                    granite:class="cmp-adaptiveform-base__enumNames"
                                                                    jcr:primaryType="nt:unstructured"
                                                                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                                    fieldDescription="Specify the content to display in the adaptive form."
                                                                    fieldLabel="Display text"
                                                                    name="./enumNames"
                                                                    required="{Boolean}false"/>
                                                                <richTextEnumNames
                                                                    jcr:primaryType="nt:unstructured"
                                                                    sling:hideResource="{Boolean}true"/>
                                                            </items>
                                                        </field>
                                                    </enumOptions>
                                                    <enumNames
                                                        granite:hidden="true"
                                                        jcr:primaryType="nt:unstructured"
                                                        sling:resourceType="granite/ui/components/coral/foundation/form/multifield">
                                                        <field
                                                            granite:class="cmp-adaptiveform-base__enumNamesHidden"
                                                            granite:hidden="true"
                                                            jcr:primaryType="nt:unstructured"
                                                            sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                            name="./enumNames"/>
                                                    </enumNames>
                                                    <areOptionsRichText
                                                        jcr:primaryType="nt:unstructured"
                                                        sling:hideResource="true"/>
                                                </items>
                                            </enums>
                                            <multiSelect
                                                granite:class="cmp-adaptiveform-dropdown__allowmultiselect"
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="saveValueType"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                name="./multiSelect"
                                                text="Allow multiple selection"
                                                value="true"/>
                                            <multiSelect-typehint
                                                jcr:primaryType="nt:unstructured"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
                                                name="./multiSelect@TypeHint"
                                                value="Boolean"/>
                                            <defaultValueSS
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="placeholder"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                fieldDescription="Specified default option is preselected on form load."
                                                fieldLabel="Default option"
                                                name="./default"
                                                wrapperClass="cmp-adaptiveform-dropdown__defaultvalue"/>
                                            <defaultValueMS
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="placeholder"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                fieldDescription="Specified default options are preselected on form load."
                                                fieldLabel="Default options"
                                                typeHint="String[]"
                                                wrapperClass="cmp-adaptiveform-dropdown__defaultvaluemultiselect">
                                                <field
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                    name="./default"
                                                    required="{Boolean}false"/>
                                            </defaultValueMS>
                                            <continent
                                                jcr:primaryType="nt:unstructured"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/select"
                                                fieldLabel="Select Continent"
                                                name="./continent"
                                                required="true">
                                                <items jcr:primaryType="nt:unstructured">
                                                    <northAmerica
                                                        jcr:primaryType="nt:unstructured"
                                                        text="North America"
                                                        value="northamerica"/>
                                                    <europe
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Europe"
                                                        value="europe"/>
                                                    <asia
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Asia"
                                                        value="asia"/>
                                                    <africa
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Africa"
                                                        value="africa"/>
                                                </items>
                                            </continent>
                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </basic>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## 後續步驟

[建立Sling模型。](./slingmodel.md)
