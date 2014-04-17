# Electoral constituencies for 2014

The data for the electoral constituencies is taken from the [Datameet github repo]
(https://github.com/datameet/maps). Please notify them of any corrections. The shp
file was loaded into the database through the **shp2pgsql** command.

```bash
$ git clone https://github.com/datameet/maps
$ cd maps/parliamentary-constituencies
$ shp2pgsql india_pc_2014.shp ge_2014 > india_pc_2014.sql
$ psql < india_pc_2014.sql
```

The [GeoJSON](http://geojson.org) file ([ge_2014.geojson]
(https://github.com/theju/ge2014_maps/tree/master/ge_2014.geojson))
has been generated on PostgreSQL 9.3 using the following SQL query:

```sql
select row_to_json(row) from (
    select 'Feature' as type,
    ST_AsGeoJSON(geom)::json as geometry,
    row_to_json(props) as properties
    from ge_2014
    inner join (select gid, st_code, st_name, pc_code, pc_name, res from ge_2014) as props
    on props.gid=ge_2014.gid
) row;
```

## Status

This is still a work in progress but feel free to check it out and contribute.

## To do

* Show a popup with the name of the constituency and a link to the [Affidavit
archive](http://affidavitarchive.nic.in/) of the Election commission of India
* HTML5 geolocation to display user's parliamentary constituency
* Link to Constituency's Wikipedia page
* Post election, name of the MP with contact details and parliament attendance
* Get state assembly data and repeat
* Display constituencies in the viewport of the map (rather than load the whole
34MB file currently).

## Credits

* The DataMeet Trust, Bangalore, India for making the data available under
the [Creative Commons Attribution-ShareAlike 2.5 India]
(http://creativecommons.org/licenses/by-sa/2.5/in/) license.
* [Leaflet](https://github.com/Leaflet/Leaflet) for making interactive maps easy!
* The PostgreSQL and PostGIS communities

## License

The data is distributed under the [Creative Commons
Attribution-ShareAlike 2.5 India](http://creativecommons.org/licenses/by-sa/2.5/in/)
license by DataMeet Trust, Bangalore, India and all other code in this repo is
distributed under the BSD 3-Clause license.

Copyright (c) 2014, Thejaswi Puthraya
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
