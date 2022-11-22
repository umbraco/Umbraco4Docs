---
versionFrom: 9.0.0
versionTo: 10.0.0
meta.Title: "Learn how to implement an Umbraco site"
meta.Description: "Get to know the Umbraco codebase. Developing an application requires knowledge about the tool you're working with. This section will give you an introduction to the structure of Umbraco."
Links-updated: partial
---
# Implementation

*Get to know the Umbraco codebase. Developing an application requires knowledge about the tool you're working with. This section will give you an introduction to the structure of Umbraco.*

<div class="row implementation">
    <div class="col-sm-12"></div>
</div>

<div class="row">
    <div class="col-xs-3 point">
    </div>
    <div class="col-xs-3">
        <span class="dot big icon-Forking">
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Default-Routing/">Default Routing</a></h4>
                <small>Describes the entire process - from a front-end user request to content delivery</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Directions-alt">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Custom-Routing/">Custom Routing</a></h4>
                <small>Custom URLs and custom MVC routes</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Circuits">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Controllers/">Controllers</a></h4>
                <small>The different type of controllers and what they can do</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Server-alt">
            <span class="line v-line top"></span>
            <span class="line h-line"></span>
            <span class="line v-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Data-Persistence/">Data Persistence</a></h4>
                <small>Manipulating Umbraco database data (CRUD)</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Globe">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Rest-Api/">REST APIs</a></h4>
                <small>(Discontinued) Information about using the REST API add-on for Umbraco</small>
            </div>
        </div>
    </div>
 </div>
 <div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Mailbox">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Composing/index">Composing</a></h4>
                <small>Customising the behaviour of an Umbraco Application at 'start up'. e.g. adding, removing or replacing the core functionality of Umbraco or registering custom code to subscribe to events.</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Server">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Services/index">Services</a></h4>
                <small>Umbraco has a range of 'Core' Services and Helpers that act as a 'gateway' to Umbraco data and functionality to use when extending or implementing an Umbraco site.</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Settings">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Unit-Testing/">Unit Testing</a></h4>
                <small>Examples of how to setup Unit Tests with Umbraco 8</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Infinity">
            <span class="line v-line top"></span>
            <span class="line v-line"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Integration-Testing/">Integration Test</a></h4>
                <small>Examples of how to setup Integration Tests with Umbraco 9.1.0</small>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-xs-3">
        <span class="dot big icon-Navigation">
            <span class="line v-line top"></span>
            <span class="line h-line"></span>
        </span>
    </div>
    <div class="col-xs-9">
        <div class="row explain">
            <div class="col-xs-12">
                <h4 class="text-right"><a href="Nullable-Reference-Types/">Nullable Reference Types</a></h4>
                <small>Using Nullable reference types, it's possible to ignore warnings or intentionally use <code>null</code> as an argument to a method expecting a non nullable reference.</small>
            </div>
        </div>
    </div>
</div>
