# Discover String Catalogs

Discover how Xcode 15 makes it easy to localize your app by managing all of your strings in one place. We’ll show you how to extract, edit, export, and build strings in your project using String Catalogs. We’ll also share how you can adopt String Catalogs in existing projects at your own pace by choosing which files to migrate.

@Metadata {
   @TitleHeading("WWDC23")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc23/10155", purpose: link, label: "Watch Video (31 min)")

   @Contributors {
      @GitHubUser(Jeehut)
   }
}



## Introduction

- Over time catalog supersedes `.strings` and `.stringsdict` files
- Strings automatically extraced by Xcode
- New keys added to all locales
- Supports parameters through String interpolation
- You can provide device-specific Strings by right-clicking (e.g. a longer text or differente term on Mac)

## Extract

- A localizable String has 4 components: key (required), default value (optional, default: key), comment (optional), table (optional, default: "Localizable")
- New single `.xcstrings` file replaces all different `en.lproj/Localizable.strings` files with no need for `.lproj` folders
- Xcode searches for localization Strings in: SwiftUI views, Storyboards, .xib files, ObjC code, C code, Shortcut phrases, `Info.plist` files
- Use `Text(..., comment: ..., bundle: ...)`
- For localizable views provided in frameworks, use `LocalizedStringResource` type -> recommended for representing and passing around
- `LocalizedStringResource` supports comment, table, default value
- You can use `String(localized:)` or `AttributedString(localized:)` (to create `LocalizedStringResource`?)
- "Use compiler to extract Swift Strings" build setting enabled by default
- Extraction woks from: `NSLocalizedString`, or any custom names provided to "LocalizedString Macro Names" build setting
- Strings in Interface Builder are localizable, too (but no way to ignore a specific one, I filed a feedback: FB12264777)
- Use `InfoPlist.xcstrings` for Info.plist -> automatic extraction
- Xcode includes big improvements for how Shortcut phrases are localized (dedicated session)
- If code provides `defaultValue:` updates, the Strings catalog updates –> code is "source of truth"
- Removed code that has no translations get removed automatically, if translation exists then marked as "Stale"

> Tip: Xcode does not automatically translate to other languages for you. You can use a drag & drop 3rd-party tool like [TranslateKit](https://translatekit.app) to do that with ease.

## Edit

- There's a way to mark a key as "Manually" managed to silence the "stale" warning
- Another red warning for "Untranslated" keys
- There's also a way to mark keys for "Needs Review" or "Reviewed"
- Pluralization is built into & simplified with substitutes like `@birds` and `@yards` when multiple params exist
- Keys you add manually (not through extraction) are marked as "Manually" managed by default
- Right-click on key and choose "Vary by plural" to turn singular String to a pluralized String

## Export

- Exporting works like before, creates an `xcloc` file which contains all localizables, includig images (standardized format)
- String Catalogs work for older OS versions as well -> they get converted to `.strings` and `.stringsdict` file on build
