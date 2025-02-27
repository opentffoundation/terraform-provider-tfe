---
layout: "tfe"
page_title: "Terraform Enterprise: tfe_team_members"
description: |-
  Manages users in a team.
---


<!-- Please do not edit this file, it is generated. -->
# tfe_team_members

Manages users in a team.

~> **NOTE** on managing team memberships: Terraform currently provides four
resources for managing team memberships.
The [tfe_team_organization_member](team_organization_member.html) and [tfe_team_organization_members](team_organization_members.html) resources are
the preferred way. The [tfe_team_member](team_member.html)
resource can be used multiple times as it manages the team membership for a
single user.  The [tfe_team_members](team_members.html) resource, on the other
hand, is used to manage all team memberships for a specific team and can only be
used once. All four resources cannot be used for the same team simultaneously.

## Example Usage

Basic usage:

```typescript
import * as constructs from "constructs";
import * as cdktf from "cdktf";
/*Provider bindings are generated by running cdktf get.
See https://cdk.tf/provider-generation for more details.*/
import * as tfe from "./.gen/providers/tfe";
class MyConvertedCode extends cdktf.TerraformStack {
  constructor(scope: constructs.Construct, name: string) {
    super(scope, name);
    const tfeTeamTest = new tfe.team.Team(this, "test", {
      name: "my-team-name",
      organization: "my-org-name",
    });
    const tfeTeamMembersTest = new tfe.teamMembers.TeamMembers(this, "test_1", {
      teamId: cdktf.Token.asString(tfeTeamTest.id),
      usernames: ["admin", "sander"],
    });
    /*This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.*/
    tfeTeamMembersTest.overrideLogicalId("test");
  }
}

```

With a set of usernames:

```typescript
import * as constructs from "constructs";
import * as cdktf from "cdktf";
/*Provider bindings are generated by running cdktf get.
See https://cdk.tf/provider-generation for more details.*/
import * as tfe from "./.gen/providers/tfe";
class MyConvertedCode extends cdktf.TerraformStack {
  constructor(scope: constructs.Construct, name: string) {
    super(scope, name);
    const allUsernames = cdktf.Fn.toset(["user1", "user2"]);
    const tfeTeamTest = new tfe.team.Team(this, "test", {
      name: "my-team-name",
      organization: "my-org-name",
    });
    const tfeTeamMembersTest = new tfe.teamMembers.TeamMembers(this, "test_1", {
      teamId: cdktf.Token.asString(tfeTeamTest.id),
      usernames: cdktf.Token.asList(
        "${[ for user in ${" + allUsernames + "} : user]}"
      ),
    });
    /*This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.*/
    tfeTeamMembersTest.overrideLogicalId("test");
  }
}

```

## Argument Reference

The following arguments are supported:

* `teamId` - (Required) ID of the team.
* `usernames` - (Required) Names of the users to add.

## Attributes Reference

* `id` - The ID of the team.

## Import

Team members can be imported; use `<TEAM ID>` as the import ID. For example:

```shell
terraform import tfe_team_members.test team-47qC3LmA47piVan7
```

<!-- cache-key: cdktf-0.17.0-pre.15 input-37e1078f8bb8f3145d8680ab69ee6750373f41a03589ff0885d16ebc69a1e13c -->