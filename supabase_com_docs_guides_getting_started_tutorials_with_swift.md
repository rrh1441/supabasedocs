Getting Started

# Build a User Management App with Swift and SwiftUI

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/supabase-swift-demo.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/swift-user-management).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#set-up-the-database-schema)

Now we are going to set up the database schema. We can use the "User Management Starter" quickstart in the SQL Editor, or you can just copy/paste the SQL from below and run it yourself.

DashboardSQL

1. Go to the [SQL Editor](https://supabase.com/dashboard/project/_/sql) page in the Dashboard.
2. Click **User Management Starter**.
3. Click **Run**.

You can easily pull the database schema down to your local project by running the `db pull` command. Read the [local development docs](https://supabase.com/docs/guides/cli/local-development#link-your-project) for detailed instructions.

`
supabase link --project-ref <project-id>
# You can get <project-id> from your project's dashboard URL: https://supabase.com/dashboard/project/<project-id>
supabase db pull
`

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#building-the-app)

Let's start building the SwiftUI app from scratch.

### Create a SwiftUI app in Xcode [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#create-a-swiftui-app-in-xcode)

Open Xcode and create a new SwiftUI project.

Add the [supabase-swift](https://github.com/supabase/supabase-swift) dependency.

Add the `https://github.com/supabase/supabase-swift` package to your app. For instructions, see the [Apple tutorial on adding package dependencies](https://developer.apple.com/documentation/xcode/adding-package-dependencies-to-your-app).

Create a helper file to initialize the Supabase client.
You need the API URL and the `anon` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#get-the-api-keys).
These variables will be exposed on the application, and that's completely fine since you have
[Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) enabled on your database.

Supabase.swift

`
import Foundation
import Supabase
let supabase = SupabaseClient(
supabaseURL: URL(string: "YOUR_SUPABASE_URL")!,
supabaseKey: "YOUR_SUPABASE_ANON_KEY"
)
`

### Set up a login view [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#set-up-a-login-view)

Set up a SwiftUI view to manage logins and sign ups.
Users should be able to sign in using a magic link.

AuthView.swift

`
import SwiftUI
import Supabase
struct AuthView: View {
@State var email = ""
@State var isLoading = false
@State var result: Result<Void, Error>?
var body: some View {
    Form {
      Section {
        TextField("Email", text: $email)
          .textContentType(.emailAddress)
          .textInputAutocapitalization(.never)
          .autocorrectionDisabled()
      }
      Section {
        Button("Sign in") {
          signInButtonTapped()
        }
        if isLoading {
          ProgressView()
        }
      }
      if let result {
        Section {
          switch result {
          case .success:
            Text("Check your inbox.")
          case .failure(let error):
            Text(error.localizedDescription).foregroundStyle(.red)
          }
        }
      }
    }
    .onOpenURL(perform: { url in
      Task {
        do {
          try await supabase.auth.session(from: url)
        } catch {
          self.result = .failure(error)
        }
      }
    })
}
func signInButtonTapped() {
    Task {
      isLoading = true
      defer { isLoading = false }
      do {
        try await supabase.auth.signInWithOTP(
            email: email,
            redirectTo: URL(string: "io.supabase.user-management://login-callback")
        )
        result = .success(())
      } catch {
        result = .failure(error)
      }
    }
}
}
`

The example uses a custom `redirectTo` URL. For this to work, add a custom redirect URL to Supabase and a custom URL scheme to your SwiftUI application. Follow the guide on [implementing deep link handling](https://supabase.com/docs/guides/auth/native-mobile-deep-linking?platform=swift).

### Account view [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#account-view)

After a user is signed in, you can allow them to edit their profile details and manage their account.

Create a new view for that called `ProfileView.swift`.

ProfileView.swift

`
import SwiftUI
struct ProfileView: View {
@State var username = ""
@State var fullName = ""
@State var website = ""
@State var isLoading = false
var body: some View {
    NavigationStack {
      Form {
        Section {
          TextField("Username", text: $username)
            .textContentType(.username)
            .textInputAutocapitalization(.never)
          TextField("Full name", text: $fullName)
            .textContentType(.name)
          TextField("Website", text: $website)
            .textContentType(.URL)
            .textInputAutocapitalization(.never)
        }
        Section {
          Button("Update profile") {
            updateProfileButtonTapped()
          }
          .bold()
          if isLoading {
            ProgressView()
          }
        }
      }
      .navigationTitle("Profile")
      .toolbar(content: {
        ToolbarItem(placement: .topBarLeading){
          Button("Sign out", role: .destructive) {
            Task {
              try? await supabase.auth.signOut()
            }
          }
        }
      })
    }
    .task {
      await getInitialProfile()
    }
}
func getInitialProfile() async {
    do {
      let currentUser = try await supabase.auth.session.user
      let profile: Profile =
      try await supabase
        .from("profiles")
        .select()
        .eq("id", value: currentUser.id)
        .single()
        .execute()
        .value
      self.username = profile.username ?? ""
      self.fullName = profile.fullName ?? ""
      self.website = profile.website ?? ""
    } catch {
      debugPrint(error)
    }
}
func updateProfileButtonTapped() {
    Task {
      isLoading = true
      defer { isLoading = false }
      do {
        let currentUser = try await supabase.auth.session.user
        try await supabase
          .from("profiles")
          .update(
            UpdateProfileParams(
              username: username,
              fullName: fullName,
              website: website
            )
          )
          .eq("id", value: currentUser.id)
          .execute()
      } catch {
        debugPrint(error)
      }
    }
}
}
`

### Models [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#models)

In `ProfileView.swift`, you used 2 model types for deserializing the response and serializing the request to Supabase. Add those in a new `Models.swift` file.

Models.swift

`
struct Profile: Decodable {
let username: String?
let fullName: String?
let website: String?
enum CodingKeys: String, CodingKey {
    case username
    case fullName = "full_name"
    case website
}
}
struct UpdateProfileParams: Encodable {
let username: String
let fullName: String
let website: String
enum CodingKeys: String, CodingKey {
    case username
    case fullName = "full_name"
    case website
}
}
`

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#launch)

Now that you've created all the views, add an entry point for the application. This will verify if the user has a valid session and route them to the authenticated or non-authenticated state.

Add a new `AppView.swift` file.

AppView.swift

`
import SwiftUI
struct AppView: View {
@State var isAuthenticated = false
var body: some View {
    Group {
      if isAuthenticated {
        ProfileView()
      } else {
        AuthView()
      }
    }
    .task {
      for await state in supabase.auth.authStateChanges {
        if [.initialSession, .signedIn, .signedOut].contains(state.event) {
          isAuthenticated = state.session != nil
        }
      }
    }
}
}
`

Update the entry point to the newly created `AppView`. Run in Xcode to launch your application in the simulator.

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like
photos and videos.

### Add `PhotosPicker` [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#add-photospicker)

Let's add support for the user to pick an image from the library and upload it.
Start by creating a new type to hold the picked avatar image:

AvatarImage.swift

`
import SwiftUI
struct AvatarImage: Transferable, Equatable {
let image: Image
let data: Data
static var transferRepresentation: some TransferRepresentation {
    DataRepresentation(importedContentType: .image) { data in
      guard let image = AvatarImage(data: data) else {
        throw TransferError.importFailed
      }
      return image
    }
}
}
extension AvatarImage {
init?(data: Data) {
    guard let uiImage = UIImage(data: data) else {
      return nil
    }
    let image = Image(uiImage: uiImage)
    self.init(image: image, data: data)
}
}
enum TransferError: Error {
case importFailed
}
`

#### Add `PhotosPicker` to profile page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-swift\#add-photospicker-to-profile-page)

ProfileView.swift

`
+ import PhotosUI
+ import Storage
+ import Supabase
import SwiftUI
struct ProfileView: View {
@State var username = ""
@State var fullName = ""
@State var website = ""
@State var isLoading = false
+ @State var imageSelection: PhotosPickerItem?
+ @State var avatarImage: AvatarImage?
var body: some View {
    NavigationStack {
      Form {
+        Section {
+          HStack {
+            Group {
+              if let avatarImage {
+                avatarImage.image.resizable()
+              } else {
+                Color.clear
+              }
+            }
+            .scaledToFit()
+            .frame(width: 80, height: 80)
+
+            Spacer()
+
+            PhotosPicker(selection: $imageSelection, matching: .images) {
+              Image(systemName: "pencil.circle.fill")
+                .symbolRenderingMode(.multicolor)
+                .font(.system(size: 30))
+                .foregroundColor(.accentColor)
+            }
+          }
+        }
        Section {
          TextField("Username", text: $username)
            .textContentType(.username)
            .textInputAutocapitalization(.never)
          TextField("Full name", text: $fullName)
            .textContentType(.name)
          TextField("Website", text: $website)
            .textContentType(.URL)
            .textInputAutocapitalization(.never)
        }
        Section {
          Button("Update profile") {
            updateProfileButtonTapped()
          }
          .bold()
          if isLoading {
            ProgressView()
          }
        }
      }
      .navigationTitle("Profile")
      .toolbar(content: {
        ToolbarItem {
          Button("Sign out", role: .destructive) {
            Task {
              try? await supabase.auth.signOut()
            }
          }
        }
      })
+      .onChange(of: imageSelection) { _, newValue in
+        guard let newValue else { return }
+        loadTransferable(from: newValue)
+      }
    }
    .task {
      await getInitialProfile()
    }
}
func getInitialProfile() async {
    do {
      let currentUser = try await supabase.auth.session.user
      let profile: Profile =
      try await supabase
        .from("profiles")
        .select()
        .eq("id", value: currentUser.id)
        .single()
        .execute()
        .value
      username = profile.username ?? ""
      fullName = profile.fullName ?? ""
      website = profile.website ?? ""
+      if let avatarURL = profile.avatarURL, !avatarURL.isEmpty {
+        try await downloadImage(path: avatarURL)
+      }
    } catch {
      debugPrint(error)
    }
}
func updateProfileButtonTapped() {
    Task {
      isLoading = true
      defer { isLoading = false }
      do {
+        let imageURL = try await uploadImage()
        let currentUser = try await supabase.auth.session.user
        let updatedProfile = Profile(
          username: username,
          fullName: fullName,
          website: website,
+          avatarURL: imageURL
        )
        try await supabase
          .from("profiles")
          .update(updatedProfile)
          .eq("id", value: currentUser.id)
          .execute()
      } catch {
        debugPrint(error)
      }
    }
}
+  private func loadTransferable(from imageSelection: PhotosPickerItem) {
+    Task {
+      do {
+        avatarImage = try await imageSelection.loadTransferable(type: AvatarImage.self)
+      } catch {
+        debugPrint(error)
+      }
+    }
+  }
+
+  private func downloadImage(path: String) async throws {
+    let data = try await supabase.storage.from("avatars").download(path: path)
+    avatarImage = AvatarImage(data: data)
+  }
+
+  private func uploadImage() async throws -> String? {
+    guard let data = avatarImage?.data else { return nil }
+
+    let filePath = "\(UUID().uuidString).jpeg"
+
+    try await supabase.storage
+      .from("avatars")
+      .upload(
+        filePath,
+        data: data,
+        options: FileOptions(contentType: "image/jpeg")
+      )
+
+    return filePath
+  }
}
`

Finally, update your Models.

Models.swift

`
struct Profile: Codable {
let username: String?
let fullName: String?
let website: String?
let avatarURL: String?
enum CodingKeys: String, CodingKey {
    case username
    case fullName = "full_name"
    case website
    case avatarURL = "avatar_url"
}
}
`

You no longer need the `UpdateProfileParams` struct, as you can now reuse the `Profile` struct for both request and response calls.

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#building-the-app) [Create a SwiftUI app in Xcode](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#create-a-swiftui-app-in-xcode) [Set up a login view](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#set-up-a-login-view) [Account view](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#account-view) [Models](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#models) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#bonus-profile-photos) [Add PhotosPicker](https://supabase.com/docs/guides/getting-started/tutorials/with-swift#add-photospicker)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings