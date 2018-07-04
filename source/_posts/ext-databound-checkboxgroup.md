---
title: EXT DataBound CheckboxGroup
tags:
  - checkboxgroup
  - databound
  - extjs
url: 2493.html
id: 2493
categories:
  - EXTjs
date: 2014-05-08 12:44:00
---

![CheckBoxGroup](/uploads/2014/04/CheckBoxGroup.png "CheckBoxGroup")Today I’ve decided to post a control I created for EXTjs that might be useful to you.  The problem I was running into was that I had a group of checkbox controls that all needed to be set based on if a record exist in the database or not.  If the record is there, the box should be checked.  If it isn’t there, it shouldn’t be checked. Further, if the user checks a box, it should add a corresponding record into the database.  Since there is nothing in the current framework that deals with this situation, I decided to create a control. The config for the groupbox is the same as the config for the Ext.form.CheckboxGroup that ships with EXT 4.2 with the addition of two additional config properties, store and inputField. The store config property lets you set the store that the control is bound to just like any other control in EXT that has a store property.  It will either use the store name, or it will use the store object. The inputField config property will let you set which field in the model should be used to set the value of the checkboxes in the group. You will configure the actual check boxes under the group as you would normally.  For the purposes of binding, you will need to set the inputValue config property to the value that will make the check box true. Here’s the code:

Ext.define('framework.form.CheckboxGroup', {
    extend: 'Ext.form.CheckboxGroup',
    alias: 'widget.boundcheckboxgroup',
    store: null,
    valueField: null,
    getBoxes: function(query) {
        return this.query('\[isCheckbox\]' \+ (query||''));
    },
    init: function () {
        var self = this;
        self.callParent(arguments);
    },
    constructor: function (configs) {
        var self = this;
        self.callParent(arguments);
        self.store = configs.store;
        self.inputField = configs.inputField;

        self.on('afterrender', function () {
            if (typeof self.store === 'string') {
                self.store = 
                    Ext.data.StoreManager.get(self.store);
            }
            if (self.store !== undefined && 
                self.store !== null) {
                Ext.Array.forEach(self.getBoxes(), 
                    function (checkbox) {
                    var record = 
                        self.store.find(self.valueField, 
                        checkbox.inputValue);
                    if (record > -1) {
                        checkbox.setValue(
                            checkbox.inputValue);
                    }
                    checkbox.on('change', 
                        function (item, newValue) {
                        record = self.store.find(
                            self.inputField, 
                            item.inputValue);
                        if (record > -1 && 
                            !newValue) {
                            self.store
                                .removeAt(record);
                        }
                        if (record === -1 
                            && newValue) {
                            var model = 
                                self.store.add({});
                            model\[0\].set(self.inputField, 
                                item.inputValue);
                        }
                    });
                });
            }
        });
    }
});
