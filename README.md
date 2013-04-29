# OpenIOC 1.1

This repo is a place to put stuff about OpenIOC 1.1 (and ultimately future versions)

We have recently released OpenIOC 1.1 internally to the IOC Stakeholders.

After this release, there was a request for a few additional modifications by stakeholders. We are posting both here, and will consolidate once we get affirmative responses from stakeholders.

## Major Changes from IOC 1.0

### OpenIOC 1.1 v1 (initial)


1. Restructured the XML. OpenIOC now has thee distinct sections

    1. metadata: Description, creator, dates... Metadata that pertains to the entire IOC.

    1. criteria: Everything that used to be `<definition>`. This is the part of the IOC that performs the actual matching.

    1. parameters: Additional section for providing arbitrary metadata on
       <Indicator> and <IndicatorItem> elements.

1. Removed IOCs requirement to respect Lucene behaviors.

1. Added additional operators. Including the existing 'is' and 'contains', we
    now have the following: The following operators are expected to perform as
    they are described in XPath 2.0. Also, OpenIOC now respects Bool & Float variables in IOC terms files.

    ```
                   String  Md5sum  IPAddr  Integer DateTime Bool  Float
    is              yes     yes     yes     yes     yes      yes   yes
    contains        yes     no      no      no      no       no    no
    matches         yes     no      yes     no      no       no    no
    starts-with     yes     no      no      no      no       no    no
    ends-with       yes     no      no      no      no       no    no
    greater-than    no      no      no      yes     yes      no    yes
    less-than       no      no      no      yes     yes      no    yes
    ```

1. Moved operator negation out of the operator itself (isnot) to its own
    attribute `IndicatorItem/@negate=true|false`.

1. Added case sensitivity `IndicatorItem/@preserve-case=true|false`.

### OpenIOC 1.1 v2 (addendums)

1. Added node context `Indicator/@node-context=xs:string`, this will allow for future ability to specify node-context to allow matches that consist of sibling or parent/child nodes that did not evaluate together properly in 1.0.

1. `IndicatorItem/@id` is now a required item. Previously it was not, allowing for Items to not have ids and still validate.

1. `IndicatorItem/content` and `IndicatorItem/context` are now `minOccurs=1` -- this prevents items from existing without content or without context (this is important now that we allow you to specify context).

1. All attributes of parameters are required to keep blank/improper parameters from being created/retained.

1. Parameters now have two id attributes: `IOCParameter/@id` is a unique GUID to identify the parameter, and `IOCParameter/@ref-id` is used to show which element inside the IOC that specific parameter refers to.

1. If you use a Link in the IOC metadata, the `rel` attribute is now required (i.e. there must actually be a link represented in the IOC xml, even if it is empty).

1. The behavior of `IndicatorItem/@preserve-case` should only apply to strings.  The table below states whether or not the given preserve-case value (true or false) is acceptable for a given content type.

    ```
                   String  Md5sum  IPAddr  Integer DateTime Bool  Float
    true            yes     no      no      no      no       no    no
    false           yes     yes     yes     yes     yes      yes   yes
    ```