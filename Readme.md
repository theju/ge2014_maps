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
archive](https://affidavitarchive.nic.in/) of the Election commission of India
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
