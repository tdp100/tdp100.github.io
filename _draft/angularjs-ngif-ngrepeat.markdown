---
layout: post
title:  "directive link scope attr is undefined?"
date:   2014-03-13 14:30:40
categories: javascript
---

##directive 标签在执行link时，发现scope中的attr为undefined？

使用ngRepeat  生成一个列表项，列表项中使用了text组件，便text组件使用ng-if会根据不同的条件显示不同的校验规则

页面的controller $scope中有校验规则对象。   当在text组件中引用controller $scope中的规则对象时，不生效

如果将规则对象放在ngRepeat item中，则会生效

````html
<div ng-repeat="configuration in values.Configurations">
    <span ng-if="configuration.cell == '%'">
        <hello-textbox id="$index"
            value="configuration.metric"
            tooltip="configuration.tooltip"
            extend-function="configuration.extendFunction"
            validate="configuration.validate"
            width="scalingpolicy.width"
            type="scalingpolicy.type">
        </hello-textbox>
    </span>
    <span ng-if="configuration.cell == 'KB/s'">
        <hello-textbox id="$index"
            value="configuration.metric"
            tooltip="configuration.Ktooltip"
            extend-function="configuration.KextendFunction"
            validate="configuration.Kvalidate"
            width="scalingpolicy.width"
            type="scalingpolicy.type">
        </hello-textbox>
    </span>
</div>
````