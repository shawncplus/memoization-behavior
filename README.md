memoization-behavior [![Build Status](https://travis-ci.org/shawncplus/memoization-behavior.svg?branch=master)](https://travis-ci.org/shawncplus/memoization-behavior) [![Published on webcomponents.org](https://img.shields.io/badge/webcomponents.org-published-blue.svg)](https://beta.webcomponents.org/element/shawncplus/memoization-behavior)

====================

`Biddle.MemoizationBehavior` allows you to define a computed property
as memoizable (http://en.wikipedia.org/wiki/Memoization) with `_memoizeComputed`
or more generally memoize any given function with `_memoize`

> Note: Methods of this behavior are "private" as a matter of convention and will
> not appear on beta.webcomponents.org hosted documentation. I've made the
> executive decision that it wouldn't be wise to allow the memoization methods
> to be published as part of an element's public API. Doing so may entice some
> hapless individual into creating some kind of element whose purpose is to
> memoize all computed properties in all elements ever.

To implement a computed property as memoizable in the simplest fashion (although
this is a trivial example, you would likely only want to memoize expensive functions)

    behaviors: [
        Biddle.MemoizationBehavior,
    ],

    properties: {
        firstName: String,
        lastName: String,
        fullName: {
            type: String,
            computed: '_memoizeComputed("fullName", firstName, lastName)'
            method: '_getFullName'
        }
    },

    _getFullName: function (first, last) {
        return first + ' ' + last;
    }

Given that the default hash function is `JSON.stringify` which may be inappropriate
for object/array parameters you may wish to override the default hashing method
for a specific property.

    properties: {
        foo: Object,
        bar: Object,
        foobar: {
            type: String,
            computed: '_memoizeComputed("fullName", foo, bar)'
            method: '_computeFoobar',
            hashFn: (foo, bar) => foo.id + ':' + bar.id
        }
    },

To generally memoize any function you may do the following

    var someValue = this._memoize(this.someExpensiveFunction)(arg2, arg2, arg3);

Cache invalidation happens manually with the `_invalidateMemoizeCache` method

    var foo = this._memoize(this.bar)('Hello');
    // ...
    this._invalidateMemoizeCache(this.bar);


