#!/usr/bin/env bash

INPUT="$*"

read -p "Enter field to be extracted: " field

FORMATTED_INPUT=$(echo "$INPUT" | sed "s/\([^ ]\+\)/'\1'/g" | sed "s/ /,/g")

ORDER_CLAUSE="CASE material "
index=1
for item in "$@"; do 
    ORDER_CLAUSE+="WHEN '$item' THEN $index "
    index=$((index+1))
done
ORDER_CLAUSE+="ELSE $index END"

QUERY="SELECT $field FROM data.material_master \
WHERE material IN ($FORMATTED_INPUT) \
ORDER BY $ORDER_CLAUSE"

echo "Query: $QUERY"

psql -U postgres -d analytics -c "COPY ($QUERY) \
TO STDOUT WITH NULL AS ''" | wl-copy 
