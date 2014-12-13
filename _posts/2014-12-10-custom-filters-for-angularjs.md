---
layout: post
title: "Custom filters for angularJS"
description: ""
category: [AngularJS]
tags: [AngularJS, filter]
---
{% include JB/setup %}

angular.module("customFilters", [])
    .filter("unique", function () {
        return function (data, propertyName) {
            if (angular.isArray(data) && angular.isString(propertyName)) {
                var results = [];
                var keys = {};
                for (var i = 0; i < data.length; i++) {
                    var val = data[i][propertyName];
                    if (angular.isUndefined(keys[val])) {
                        keys[val] = true;
                        results.push(val);
                    }
}
                return results;
            } else {
                return data;
            }
} });


 range, returns a range of elements from an array, corresponding to a page of products. The filter accepts arguments for the currently selected page (which is used to determine the start index of range) and the page size (which is used to determine the end index).

 .filter("range", function ($filter) {
    return function (data, page, size) {
        if (angular.isArray(data) && angular.isNumber(page) && angular.isNumber(size)) {
            var start_index = (page - 1) * size;
            if (data.length < start_index) {
                return [];
            } else {
                return $filter("limitTo")(data.splice(start_index), size);
            }
        } else {
            return data;
        }
    }
})

