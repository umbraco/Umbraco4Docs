---
versionFrom: 7.0.0
versionRemoved: 8.0.0
---

# Recursively select a property

Sometimes in your XSLT you need to access the data not on the `$currentPage` but possibly any parent nodes.

For example using the related links data type to output a list of links on the currentPage works OK if I have that property on that page

    <xsl:for-each select="$currentPage/data [@alias='links']//link>
        ...
    </xsl:for-each>

However if I have this XSLT macro in a masterpage within a sidebar and navigate down to a child page, the above XSLT would not work unless I have that property on that child node. So to solve this I have modified the XPath.

    <xsl:for-each select="$currentPage/ancestor-or-self::node/data[@alias='links' !=''][1]//link">
        ...
    </xsl:for-each>

$currentPage/ancestor-or-self::node/data[@alias='links' !=''][1]

What this new XPath is doing is saying is: Walk up the tree until you find a node with a property called links that is not empty and then Select the first item. This would be the lowest item when walking up the tree.
