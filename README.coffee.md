A Knockout Widget for CCNQ4 gwlist
----------------------------------

    module.exports = (require 'ccnq-ko') 'rule-gwlist', (ko) ->

Data
----

      @data class RuleGwlist
        constructor: (gwlist) ->
          gwlist ?= []

          @gwlist = ko.observableArray gwlist.map (data) ->
            new RuleTarget data
          return

        toJS: ->
          @gwlist.remove (x) -> !x._validated()
          ko.toJS @gwlist

Model
-----

      @view ({value,ko}) ->
        # assert value instanceof RuleGwlist, 'value should be an instance of RuleGwlist'
        assert value?, 'value is required'

        @notify = ko.observable ''

Add a new (empty) target.
FIXME: should select a default type based on existing targets (e.g. only one `source_registrant` makes sense).

        @add_gw = =>
          @gwlist.push new RuleTarget {}

Remove an existing target.

        @remove_gw = (target) =>
          @gwlist.remove target

        return

Layout
------

      @html ({p,div,text,label,input,datalist,option,ul,li,button,tag}) ->

        datalist '#gateway', bind: foreach: '$root.gateways', ->
          option bind: value: '$data'
        datalist '#carrier', bind: foreach: '$root.carriers', ->
          option bind: value: '$data'

        div bind: foreach: 'gwlist', ->
          div ->
            rule_target '$data'
            button bind: click: '$parent.remove_gw', 'Remove'
        button bind: click: 'add_gw', 'Add'

Extend Knockout witht the `rule-target` component/tag.

      {RuleTarget,rule_target} = (require 'ccnq-ko-rule-target') ko

    assert = require 'assert'
