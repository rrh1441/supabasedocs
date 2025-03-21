REST API

# Converting SQL to JavaScript API

* * *

Many common SQL queries can be written using the JavaScript API, provided by the SDK to wrap Data API calls. Below are a few examples of conversions between SQL and JavaScript patterns.

## Select statement with basic clauses [\#](https://supabase.com/docs/guides/api/sql-to-api\#select-statement-with-basic-clauses)

Select a set of columns from a single table with where, order by, and limit clauses.

`
select first_name, last_name, team_id, age
from players
where age between 20 and 24 and team_id != 'STL'
order by last_name, first_name desc
limit 20;
`

`
const { data, error } = await supabase
.from('players')
.select('first_name,last_name,team_id,age')
.gte('age', 20)
.lte('age', 24)
.not('team_id', 'eq', 'STL')
.order('last_name', { ascending: true }) // or just .order('last_name')
.order('first_name', { ascending: false })
.limit(20)
`

## Select statement with complex Boolean logic clause [\#](https://supabase.com/docs/guides/api/sql-to-api\#select-statement-with-complex-boolean-logic-clause)

Select all columns from a single table with a complex where clause: OR AND OR

`
select *
from players
where ((team_id = 'CHN' or team_id is null) and (age > 35 or age is null));
`

`
const { data, error } = await supabase
.from('players')
.select() // or .select('*')
.or('team_id.eq.CHN,team_id.is.null')
.or('age.gt.35,age.is.null') // additional filters imply "AND"
`

Select all columns from a single table with a complex where clause: AND OR AND

`
select *
from players
where ((team_id = 'CHN' and age > 35) or (team_id != 'CHN' and age is not null));
`

`
const { data, error } = await supabase
.from('players')
.select() // or .select('*')
.or('and(team_id.eq.CHN,age.gt.35),and(team_id.neq.CHN,.not.age.is.null)')
`

## Resources [\#](https://supabase.com/docs/guides/api/sql-to-api\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [PostgREST Operators](https://postgrest.org/en/stable/api.html#operators)
- [Supabase API: JavaScript select](https://supabase.com/docs/reference/javascript/select)
- [Supabase API: JavaScript modifiers](https://supabase.com/docs/reference/javascript/using-modifiers)
- [Supabase API: JavaScript filters](https://supabase.com/docs/reference/javascript/using-filters)

### Is this helpful?

NoYes

### On this page

[Select statement with basic clauses](https://supabase.com/docs/guides/api/sql-to-api#select-statement-with-basic-clauses) [Select statement with complex Boolean logic clause](https://supabase.com/docs/guides/api/sql-to-api#select-statement-with-complex-boolean-logic-clause) [Resources](https://supabase.com/docs/guides/api/sql-to-api#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings