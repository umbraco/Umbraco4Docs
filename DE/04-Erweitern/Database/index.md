---
versionFrom: 9.0.0
versionTo: 10.0.0
meta.Title: "Umbraco Database"
meta.Description: "A guide to creating a custom Database table in Umbraco"
---

# Creating a custom Database table

In Umbraco it is possible to add custom database tables to your site if you want to store additional data that should not be stored as normal content nodes.

If migrating from v8, you'll be able to use a similar method as was available in that version.  You register a component in a composer, create a migration plan and run the plan to add the database table to the database. Learn more about composers in the [Composing](../../Implementation/Composing/) article.

The end result looks like this:

![Database result of a migration](images/db-table.png)

## Using a composer and component

The following code sample shows how this is done in Umbraco v9.  If migrating from v8, the only changes to note other than namespace updates, are the dependencies that need to be passed to the `Upgrader.Execute()` method, and a change to the access modifier of the `Migrate()` method.

```csharp
using Microsoft.Extensions.Logging;
using NPoco;
using Umbraco.Cms.Core;
using Umbraco.Cms.Core.Composing;
using Umbraco.Cms.Core.Migrations;
using Umbraco.Cms.Core.Scoping;
using Umbraco.Cms.Core.Services;
using Umbraco.Cms.Infrastructure.Migrations;
using Umbraco.Cms.Infrastructure.Migrations.Upgrade;
using Umbraco.Cms.Infrastructure.Persistence.DatabaseAnnotations;

namespace MyNamespace
{
    public class BlogCommentsComposer : ComponentComposer<BlogCommentsComponent>, IComposer
    {
    }

    public class BlogCommentsComponent : IComponent
    {
        private readonly IScopeProvider _scopeProvider;
        private readonly IMigrationPlanExecutor _migrationPlanExecutor;
        private readonly IKeyValueService _keyValueService;
        private readonly IRuntimeState _runtimeState;

        public BlogCommentsComponent(
            IScopeProvider scopeProvider,
            IMigrationPlanExecutor migrationPlanExecutor,
            IKeyValueService keyValueService,
            IRuntimeState runtimeState)
        {
            _scopeProvider = scopeProvider;
            _migrationPlanExecutor = migrationPlanExecutor;
            _keyValueService = keyValueService;
            _runtimeState = runtimeState;
        }

        public void Initialize()
        {
            if (_runtimeState.Level < RuntimeLevel.Run)
            {
                return;
            }

            // Create a migration plan for a specific project/feature
            // We can then track that latest migration state/step for this project/feature
            var migrationPlan = new MigrationPlan("BlogComments");

            // This is the steps we need to take
            // Each step in the migration adds a unique value
            migrationPlan.From(string.Empty)
                .To<AddCommentsTable>("blogcomments-db");

            // Go and upgrade our site (Will check if it needs to do the work or not)
            // Based on the current/latest step
            var upgrader = new Upgrader(migrationPlan);
            upgrader.Execute(_migrationPlanExecutor, _scopeProvider, _keyValueService);
        }

        public void Terminate()
        {
        }
    }

    public class AddCommentsTable : MigrationBase
    {
        public AddCommentsTable(IMigrationContext context) : base(context)
        {
        }
        protected override void Migrate()
        {
            Logger.LogDebug("Running migration {MigrationStep}", "AddCommentsTable");

            // Lots of methods available in the MigrationBase class - discover with this.
            if (TableExists("BlogComments") == false)
            {
                Create.Table<BlogCommentSchema>().Do();
            }
            else
            {
                Logger.LogDebug("The database table {DbTable} already exists, skipping", "BlogComments");
            }
        }

        [TableName("BlogComments")]
        [PrimaryKey("Id", AutoIncrement = true)]
        [ExplicitColumns]
        public class BlogCommentSchema
        {
            [PrimaryKeyColumn(AutoIncrement = true, IdentitySeed = 1)]
            [Column("Id")]
            public int Id { get; set; }

            [Column("BlogPostUmbracoId")]
            public int BlogPostUmbracoId { get; set; }

            [Column("Name")]
            public string Name { get; set; }

            [Column("Email")]
            public string Email { get; set; }

            [Column("Website")]
            public string Website { get; set; }

            [Column("Message")]
            [SpecialDbType(SpecialDbTypes.NVARCHARMAX)]
            public string Message { get; set; }
        }
    }
}
```

## Using a notification handler

If building a new solution in Umbraco V9, you can adopt a new pattern, where you create and run a similar migration but trigger it in response to a [notification handler](../../Fundamentals/Code/Subscribing-To-Notifications\index.md).

The code for this approach is as follows:

```C#
using Microsoft.Extensions.Logging;
using NPoco;
using Umbraco.Cms.Core;
using Umbraco.Cms.Core.Events;
using Umbraco.Cms.Core.Migrations;
using Umbraco.Cms.Core.Notifications;
using Umbraco.Cms.Core.Scoping;
using Umbraco.Cms.Core.Services;
using Umbraco.Cms.Infrastructure.Migrations;
using Umbraco.Cms.Infrastructure.Migrations.Upgrade;
using Umbraco.Cms.Infrastructure.Persistence.DatabaseAnnotations;

namespace MyNamespace
{
    public class RunBlogCommentsMigration : INotificationHandler<UmbracoApplicationStartingNotification>
    {
        private readonly IMigrationPlanExecutor _migrationPlanExecutor;
        private readonly IScopeProvider _scopeProvider;
        private readonly IKeyValueService _keyValueService;
        private readonly IRuntimeState _runtimeState;

        public RunBlogCommentsMigration(
            IScopeProvider scopeProvider,
            IMigrationPlanExecutor migrationPlanExecutor,
            IKeyValueService keyValueService,
            IRuntimeState runtimeState)
        {
            _migrationPlanExecutor = migrationPlanExecutor;
            _scopeProvider = scopeProvider;
            _keyValueService = keyValueService;
            _runtimeState = runtimeState;
        }

        public void Handle(UmbracoApplicationStartingNotification notification)
        {
            if (_runtimeState.Level < RuntimeLevel.Run)
            {
                return;
            }

            // Create a migration plan for a specific project/feature
            // We can then track that latest migration state/step for this project/feature
            var migrationPlan = new MigrationPlan("BlogComments");

            // This is the steps we need to take
            // Each step in the migration adds a unique value
            migrationPlan.From(string.Empty)
                .To<AddCommentsTable>("blogcomments-db");

            // Go and upgrade our site (Will check if it needs to do the work or not)
            // Based on the current/latest step
            var upgrader = new Upgrader(migrationPlan);
            upgrader.Execute(
                _migrationPlanExecutor,
                _scopeProvider,
                _keyValueService);
        }
    }

    // Migration and schema defined as in the previous code sample.
}
```

The notification handler can either be registered in a composer:

```C#
using Umbraco.Cms.Core.Composing;
using Umbraco.Cms.Core.DependencyInjection;
using Umbraco.Cms.Core.Notifications;

namespace TableMigrationTest
{
    public class BlogCommentsComposer : IComposer
    {
        public void Compose(IUmbracoBuilder builder)
        {
            builder.AddNotificationHandler<UmbracoApplicationStartingNotification, RunBlogCommentsMigration>();
        }
    }
}
```

Or in an extension method called from `StartUp.cs` as is preferred:

```C#
using System.Linq;
using Umbraco.Cms.Core.DependencyInjection;
using Umbraco.Cms.Core.Notifications;

namespace MyNamespace
{
    public static class UmbracoBuilderExtensions
    {
        public static IUmbracoBuilder AddBlogComments(this IUmbracoBuilder builder)
        {
            builder.AddNotificationHandler<UmbracoApplicationStartingNotification, RunBlogCommentsMigration>();
            return builder;
        }
    }
}
```

```C#
using System;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Umbraco.Cms.Core.DependencyInjection;
using Umbraco.Extensions;

namespace MyNamespace
{
    public class Startup
    {
        ...

        public void ConfigureServices(IServiceCollection services)
        {
            services.AddUmbraco(_env, _config)
                .AddBackOffice()
                .AddWebsite()
                .AddComposers()
                .AddBlogComments()  // calls our extension method to register the notification handler
                .Build();

        }

        ...
    }
}

```

## Which to use?

In short, it's up to you.  If you are migrating from V8 and want the quickest route to getting running with V9, then using a component makes sense.

With V9 you will likely find you are using the notification pattern elsewhere, such as when responding to Umbraco events that run many times in the lifetime of the application, like when content is saved.  And so you may also prefer to align with that pattern for start-up events.

It's also worth noting that components offer both `Initialize` and `Terminate` methods, where you will need to handle two notifications to do the same with the notification handler approach (`UmbracoApplicationStartingNotification` and `UmbracoApplicationStoppingNotification`).  A single handler class can be used for both notifications though.

## Schema class and migrations

**Important!** It is important to note that the `BlogCommentSchema` class nested inside the migration is purely used as a database schema representation class and should not be used as a Data Transfer Object (DTO) to access the table data. Equally, you shouldn't use your DTO classes to define the schema used by your migration. Instead you should create a duplicate snapshot as demonstrated above specifically for the purpose of creating or working with your database tables in the current migration. The name of the class is not important as you will be overriding it using the TableName attribute. So you should choose a name that makes it clear for you and everyone else that this class is purely for defining the schema in this migration.

Whilst this adds a level of duplication, it is important that migrations and the code/classes within a migration remain immutable. If the DTO was to be used for both, it could cause unexpected behaviour should you later modify your DTO used in your application but you have previous migrations expecting the DTO to be in its unmodified state.

Once a snapshot has been created, and once your code has been deployed, the snapshot should never be changed directly. Instead, you should use further migrations to alter the database table into the state you require. This ensures that migrations can always be run in sequence and that each migration can expect the database to be in a known state before executing.

When adding further migrations it is also important to note that if you need to reuse the schema class, it can be a good idea to duplicate this again in those particular migrations. You want the migrations to be immutable, so having separate classes in separate namespaces, reduces the risk of modifying a schema class from your initial migration.

## Data stored in custom database tables

When storing data in custom database tables, this is by default not manageable by Umbraco at all. This can be great for many purposes such as storing massive amounts of data that you do not need to edit from inside the Umbraco backoffice. Decoupling part of your data from being managed by Umbraco as content can be a way of achieving better performance for your site. That way, it will no longer take up space in indexes and caches, and the Umbraco database which may not have the best structure for your type of data.

This however also means that if you do need to edit or display this data, it is up to you to implement the underlying functionality supporting this.

It also means that if you need this data to be transferred or kept synchronized between multiple sites or environments, it is up to you to handle this. Data stored in custom tables are not supported out of the box by add-ons such as Umbraco Deploy or Umbraco Courier and therefore will not be deployable by default.

Figuring out how to manage data across multiple environments can be very different between individual sites and there is not one solution that fits all. Some sites may have automated database synchronization set up to ensure specific tables in multiple databases are always kept in sync, while others may be better off with scripts moving data around manually on demand.
