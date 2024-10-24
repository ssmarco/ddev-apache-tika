[![tests](https://github.com/ssmarco/ddev-apache-tika/actions/workflows/tests.yml/badge.svg)](https://github.com/ssmarco/ddev-apache-tika/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2024.svg)

# ddev-apache-tika <!-- omit in toc -->

- [Introduction](#introduction)
- [Getting started](#getting-started)
- [How to debug in Github Actions](#how-to-debug-tests-github-actions)

## Introduction

ddev-apache-tika is an un-official implementation of Apache Tika service for DDEV based on their Docker guide\*.

From your DDEV project, install this by running `ddev get ssmarco/ddev-apache-tika` followed by `ddev restart`.

- [Docker guide\*](https://github.com/apache/tika-docker)

## Getting started

1. In the DDEV project directory:

    For DDEV v1.23.5 or above run

    ```sh
    ddev add-on get ssmarco/ddev-apache-tika
    ```

    For earlier versions of DDEV run

    ```sh
    ddev get ssmarco/ddev-apache-tika
    ```

2. Restart the DDEV instance:

    ```sh
    ddev restart
    ```

3. Get the URL of the Kibana dashboard (e.g. https://your-project-name.ddev.site:5602):

    ```sh
    ddev describe
    ```

## Configuring your framework

### Silverstripe

1. Update your project's `.env` file. The API keys are found in the Enterprise Search section of Kibana dashboard.

    ```
    SS_TIKA_ENDPOINT="http://tika:9998"
    ```

2. The Apache Tika endpoint is `http://tika:9998`

3. The following modules are tested to work out of the box in your composer.json file:

    ```
    "silverstripe/silverstripe-textextraction": "^4"
    ```

## Troubleshooting

1. Make sure all required containers are downloaded

    ```sh
    docker pull apache/tika:latest
    ```

2. Remove container volumes to restart from scratch

    List all existing volumes from your system:

    ```sh
    docker volume ls
    ```

    This will show example output below:

    ```
    DRIVER    VOLUME NAME
    local     ddev-your-project-name_tika
    ```

    Delete the volumes by running:

    ```sh
    docker volume rm ddev-your-project-name_tika
    ```

3. Restart by `ddev restart`

4. Check the status of the project by `ddev status`

5. Check the logs

    ```sh
    ddev logs -s tika
    ```

6. Check job health

    You might need to install `jq` for better legibility of the output.

    ```sh
    docker inspect --format "{{json .State.Health }}" ddev-your-project-name-tika | jq
    ```

7. Check memory consumptions

    ```sh
    docker stats
    ```

## Warning

This is for local development purposes only. Testing large amount of data depends on the host computer's resources.

If you have a good amount of CPU's and memory, you can increase the value of `mem_limit` for each container or remove this attribute to assign more resources as needed.

## Contribute

- Anyone is welcome to submit a PR to this repo. See README.md at https://github.com/ddev/ddev-addon-template, the parent of this repo.

## Maintainer

- Contributed and maintained by [Marco Hermo](https://github.com/ssmarco).
