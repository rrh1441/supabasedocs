Getting Started

# Use Supabase with refine

## Learn how to create a Supabase project, add some sample data to your database, and query the data from a refine app.

* * *

1

### Create a Supabase project

Go to [database.new](https://database.new/) and create a new Supabase project.

When your project is up and running, go to the [Table Editor](https://supabase.com/dashboard/project/_/editor), create a new table and insert some data.

Alternatively, you can run the following snippet in your project's [SQL Editor](https://supabase.com/dashboard/project/_/sql/new). This will create a `instruments` table with some sample data.

SQL\_EDITOR

`
-- Create the table
create table instruments (
id bigint primary key generated always as identity,
name text not null
);
-- Insert some sample data into the table
insert into instruments (name)
values
('violin'),
('viola'),
('cello');
alter table instruments enable row level security;
`

Make the data in your table publicly readable by adding an RLS policy:

SQL\_EDITOR

`
create policy "public can read instruments"
on public.instruments
for select to anon
using (true);
`

2

### Create a refine app

Create a [refine](https://github.com/refinedev/refine) app using the [create refine-app](https://refine.dev/docs/getting-started/quickstart/).

The `refine-supabase` preset adds `@refinedev/supabase` supplementary package that supports Supabase in a refine app. `@refinedev/supabase` out-of-the-box includes the Supabase dependency: [supabase-js](https://github.com/supabase/supabase-js).

Terminal

`
npm create refine-app@latest -- --preset refine-supabase my-app
`

3

### Open your refine app in VS Code

You will develop your app, connect to the Supabase backend and run the refine app in VS Code.

Terminal

`
cd my-app
code .
`

4

### Start the app

Start the app, go to [http://localhost:5173](http://localhost:5173/) in a browser, and you should be greeted with the refine Welcome page.

Terminal

`
npm run dev
`

![refine welcome page](https://supabase.com/docs/img/refine-qs-welcome-page.png)

5

### Update \`supabaseClient\`

You now have to update the `supabaseClient` with the `SUPABASE_URL` and `SUPABASE_KEY` of your Supabase API. The `supabaseClient` is used in auth provider and data provider methods that allow the refine app to connect to your Supabase backend.

###### Project URL

No project found

To get your Project URL, [log in](https://supabase.com/dashboard).

###### Anon key

No project found

To get your Anon key, [log in](https://supabase.com/dashboard).

src/utility/supabaseClient.ts

`
import { createClient } from "@refinedev/supabase";
const SUPABASE_URL = YOUR_SUPABASE_URL;
const SUPABASE_KEY = YOUR_SUPABASE_KEY
export const supabaseClient = createClient(SUPABASE_URL, SUPABASE_KEY, {
db: {
    schema: "public",
},
auth: {
    persistSession: true,
},
});
`

6

### Add instruments resource and pages

You have to then configure resources and define pages for `instruments` resource.

Use the following command to automatically add resources and generate code for pages for `instruments` using refine Inferencer.

This defines pages for `list`, `create`, `show` and `edit` actions inside the `src/pages/instruments/` directory with `<HeadlessInferencer />` component.

The `<HeadlessInferencer />` component depends on `@refinedev/react-table` and `@refinedev/react-hook-form` packages. In order to avoid errors, you should install them as dependencies with `npm install @refinedev/react-table @refinedev/react-hook-form`.

The `<HeadlessInferencer />` is a refine Inferencer component that automatically generates necessary code for the `list`, `create`, `show` and `edit` pages.

More on [how the Inferencer works is available in the docs here](https://refine.dev/docs/packages/documentation/inferencer/).

Terminal

`
npm run refine create-resource instruments
`

7

### Add routes for instruments pages

Add routes for the `list`, `create`, `show`, and `edit` pages.

You should remove the `index` route for the Welcome page presented with the `<Welcome />` component.

src/App.tsx

`
import { Refine, WelcomePage } from "@refinedev/core";
import { RefineKbar, RefineKbarProvider } from "@refinedev/kbar";
import routerBindings, {
DocumentTitleHandler,
NavigateToResource,
UnsavedChangesNotifier,
} from "@refinedev/react-router-v6";
import { dataProvider, liveProvider } from "@refinedev/supabase";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import "./App.css";
import authProvider from "./authProvider";
import { supabaseClient } from "./utility";
import { InstrumentsCreate, InstrumentsEdit, InstrumentsList, InstrumentsShow } from "./pages/instruments";
function App() {
return (
    <BrowserRouter>
      <RefineKbarProvider>
        <Refine
          dataProvider={dataProvider(supabaseClient)}
          liveProvider={liveProvider(supabaseClient)}
          authProvider={authProvider}
          routerProvider={routerBindings}
          options={{
            syncWithLocation: true,
            warnWhenUnsavedChanges: true,
          }}
          resources={[{\
            name: "instruments",\
            list: "/instruments",\
            create: "/instruments/create",\
            edit: "/instruments/edit/:id",\
            show: "/instruments/show/:id"\
          }]}>
          <Routes>
            <Route index
              element={<NavigateToResource resource="instruments" />}
            />
            <Route path="/instruments">
              <Route index element={<InstrumentsList />} />
              <Route path="create" element={<InstrumentsCreate />} />
              <Route path="edit/:id" element={<InstrumentsEdit />} />
              <Route path="show/:id" element={<InstrumentsShow />} />
            </Route>
          </Routes>
          <RefineKbar />
          <UnsavedChangesNotifier />
          <DocumentTitleHandler />
        </Refine>
      </RefineKbarProvider>
    </BrowserRouter>
);
}
export default App;
`

8

### View instruments pages

Now you should be able to see the instruments pages along the `/instruments` routes. You may now edit and add new instruments using the Inferencer generated UI.

The Inferencer auto-generated code gives you a good starting point on which to keep building your `list`, `create`, `show` and `edit` pages. They can be obtained by clicking the `Show the auto-generated code` buttons in their respective pages.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings