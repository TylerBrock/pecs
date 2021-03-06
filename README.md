PECS: Flex your ECS muscles
===========================

PECS allows you to easily deploy your docker projects to AWS's ECS platform.

### Rationale

**Q:** Why would we build another ECS deploy tool when many already exist?

**A:** PECS supports deploying an image to multiple services at once.

ECS users often have multiple services that share some code and use
the same image but be run with different commands or entrypoints.

For example, a single image could be used for an api service and a worker service.

 - When you update that image, it would be nice to update multiple services
along with it.
 - Likewise, when you roll back a release, it's nice to be able
to roll back all related services at once.


Diagram of staging cluster that has many services all backed by the same image:
```
          -> api (backend:123)
         /
staging -> worker (backend:123)
         \
          -> web (backend:123)
```

### Dependencies

Currently requires that AWS is configured via the environment:
  - `AWS_ACCESS_KEY_ID` must be set
  - `AWS_SECRET_ACCESS_KEY` must be set

Requires Node 4+

### Installation ###

```sh
npm install -g pecs
```

### Usage ###

```
pecs <command>

Commands:
  pecs clusters                Get list of clusters
  pecs services                Get list of services in a cluster
  pecs updateAgents            Update all ECS agents in a cluster
  pecs release                 Update service(s) with new image
  pecs rollback                Roll back service(s)
  pecs bump                    Rolling restart a service across a cluster
  pecs config [get|set|unset]  View or modify service environments
```

#### Release ####
```
pecs release

Update service(s) with new image

Examples:
  pecs release -c dev -s api            update dev api service
  pecs release -c dev -s api worker     update dev api + worker services
  pecs release -c dev -s api -t v1.2.3  update dev api to v1.2.3
```

#### Rollback ####
```
pecs rollback

Roll back service(s)

Examples:
  pecs rollback -c dev -s api           roll back api to previous task def
  pecs rollback -c dev -s api worker    roll back api + worker
  pecs rollback -c dev -s api --rev -2  roll back api 2 release ago
```

#### Bump ####
```
pecs bump

Rolling restart a service across a cluster

Examples:
  pecs config -c dev         restart all development containers
  pecs config -c dev -s api  restart development api containers
```

#### Config ####
```
pecs config [get|set|unset]

View or modify service environments

Commands:
  pecs config get <key>        Get environment variable for a service
  pecs config set <key> <val>  Set environment variable for a service
  pecs config unset <key>      Unset environment variable for a service

Examples:
  pecs config -c dev  get all dev cluster env vars
```

### Development

#### Setup ####

```sh
git clone git@github.com:HustleInc/pecs.git
```

#### Run in Node 8+ ####

```
node src/pecs.js
```

#### Active Dev (Transpiled) ####

```sh
npm link
```

Run the dev mode to constantly transpile the source
```sh
npm run dev
```

Run pecs
```sh
pecs
```
