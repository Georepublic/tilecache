Tile Cache recipe for tile.openstreetmap.jp
=========

Author: Hiroshi Miura, OpenStreetMap Foundation Japan <miurahr@osmf.jp>


Here is a repository to maintain tile.openstreetmap.jp tile cache/tile server.
It uses following technologies.

- Nginx Web server (1.3.12 or over)

  - Lua embeded scripting 

  - File Cache

- Tirex, rendering backend

- PostGIS/postgresql 9.1

- osm2pgsql

- osmosis (latest)

This recipe is intended to run on Ubuntu 11.10(x86_64) server but it may be
useful for other platform and who want to run osm tile server.


Install
--
[Setup development environment using Vagrant](https://github.com/osmfj/tilecache/wiki/Setup-development-environment-using-Vagrant)

License
-- 

The nginx recipe and lua script is distributed under AGPLv3.
render_expire is made by Frederik Ramm <frederik@remote.org> and distirbuted
by GPLv2.
Each softwares are under the each licenses.

Maintainer
--

It is maintained by OpenStreetMap Foundation Japan.


Design
==

Nginx serves tile proxy. It returns disk cache and escalate to upstream
tile.openstreetmap.org servers when needed.
Lua script included by Nginx controls local rendering.
It is an asumption that postgis server has limited osm data in region.

Lua script retrive x/y/z parameter and check an existence of 
tile data. If it is out of area where the server provided, it goes upstream.

We need another script to maintain tile generation control.
We can get expire.list as "Tile expire method" explaines when importing diff.osm.
http://wiki.openstreetmap.org/wiki/Tile_expire_methods


planet import
---

The directory updatedb has an incremental update script and primary load script
for osm data.
It is now defaults geofabrik data and also supposed to use planet.osm.org data. 


Data
====

This distribution includes several tile images.

It can be used to replace some tiles,  where some country law request to 
display specific name.

There are several places where multiple laws in countries requires incoherent 
rules, such as administration claims.

A nginx configuration, statictile provides a solution for these case.
For details, please refer doc/statictile.ja.txt


Reference
--

- http://svn.openstreetmap.org/applications/utils/tirex/tileserver/tileserver.js

- http://wiki.openstreetmap.org/wiki/User:Stephankn/knowledgebase

- https://launchpad.net/~nginx/+archive/development

