# Deploy and edit your travel map

The public website is view-only. You edit one file in your GitHub repository, while signed into your own GitHub account. Visitors have no editing controls, and cannot save changes to your site. Only you and collaborators you explicitly grant repository write access can publish changes. The published stories, photographs, and content file are publicly readable.

## 1. Put the map on your website

1. Download **map-github.zip** and unzip it. Do not upload the ZIP itself.
2. Sign into GitHub as **potterisom**. Create a repository named **map**. A public repository is the straightforward option for GitHub Pages. If that repository already exists, use it instead.
3. Inside the repository, choose **Add file → Upload files**. Drag in the **contents** of the unzipped folder. Upload `index.html`, `content.js`, `assets/`, `photos/`, the guide files and `.nojekyll`.
4. Make sure `index.html` is at the top level of the repository, not inside an extra `map-github` folder. Keep filenames and folder names unchanged.
5. Click **Commit changes** and save to `main`.
6. Open **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**. Choose **main** and **/(root)**, then **Save**.
7. GitHub will show the published address on that settings page. For the `potterisom/map` repository it should be `https://potterisom.github.io/map/`. Publication can take up to 10 minutes.
8. Add a link to `/map/` from your main website's navigation when you're ready.

If your computer hides `.nojekyll`, create it on GitHub using **Add file → Create new file**, name it `.nojekyll`, and commit it. It can be empty.

No terminal commands, package installation, API keys, custom Actions workflow, or local build are needed for this upload package.

Official instructions: [GitHub Pages publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site), [creating a Pages site](https://docs.github.com/articles/creating-project-pages-manually).

## 2. The files you will actually edit

| File or folder | Purpose |
| --- | --- |
| `content.js` | City names, coordinates, descriptions, photographs, country statuses and optional tour order |
| `photos/` | Your image files; city subfolders are optional |
| `EDITING-GUIDE.md` | These instructions |
| `COUNTRY-NAMES.md` | Exact country names supported by the map |
| `index.html` and `assets/` | The viewer; leave these alone for content changes |

## 3. Add or edit a city description

1. Open **content.js** on GitHub.
2. Click its **pencil / Edit this file** button.
3. Find the city's `id`, for example `oxford`.
4. Replace its empty `story: `` ,` field with text **between the two backticks**. Keep the comma after the closing backtick.

Example:

```js
story: `Write your first paragraph here.

Write your second paragraph here. You can use ordinary quotation marks and apostrophes.`,
```

5. Click **Commit changes**, enter a short message such as `Add Oxford story`, and commit to `main`.
6. After GitHub finishes publishing, refresh the map and open Oxford.

Stories are plain text. Blank lines create paragraph spacing. Markdown and HTML are not interpreted. To include a literal backtick inside a story, put a backslash before it. To include the literal sequence `${`, write `\${`. Leave the surrounding braces and commas in place.

## 4. Add photographs to a city

1. On your computer, rename images with simple lowercase names, such as `oxford-library.jpg`. Avoid spaces. Supported formats are JPG/JPEG, PNG, WebP, AVIF and GIF; convert HEIC images to JPG first.
2. In the GitHub repository, open **photos/**, choose **Add file → Upload files**, upload the image files, then **Commit changes**. You can also upload a folder, e.g. `photos/oxford/`.
3. Edit the city's `photos: []` in **content.js**:

```js
photos: [
  {
    src: "photos/oxford-library.jpg",
    alt: "Describe what is visible in this photograph.",
    caption: "Write an optional caption here."
  },
  {
    src: "photos/oxford/another-photo.jpg",
    alt: "Describe the second photograph."
  }
],
```

4. Replace those example paths with the **exact names of files you actually uploaded**. Paths are case-sensitive. Use `photos/...`, with no slash at the start.
5. Commit the content change. The photos appear in the same order as the list. `alt` is required; `caption` is optional. To reorder photographs, reorder the objects. To remove one from the page, delete its object from the list.

For faster pages, resize large photographs to around 1600–2000 pixels wide and prefer reasonably compressed files. The viewer loads images as needed. [GitHub's browser uploader](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository) accepts individual files up to 25 MiB; smaller photos are preferable.

## 5. Add a city

Add one object inside the `cities: [ ... ]` list in **content.js**. Keep a comma between objects. This Paris entry is an example, not a claim that you have visited Paris:

```js
{
  id: "paris",
  name: "Paris",
  country: "France",
  lat: 48.8566,
  lon: 2.3522,
  story: `Write your own description here.`,
  photos: []
},
```

- **id:** A unique stable identifier. Use lowercase letters, numbers and underscores, e.g. `san_francisco`. Keep the ID unchanged when you rename a city so existing links continue to work.
- **name:** The city name displayed to visitors.
- **country:** Use a country name from `COUNTRY-NAMES.md`, or the name already used in your `countries` list.
- **lat / lon:** Decimal latitude and longitude as numbers. South and west are negative. Latitude comes first; don't put degree symbols or quotes around the numbers. Copy coordinates from a map service, rather than guessing.
- **story / photos:** Your text and photographs. Empty values are allowed.
- **heart:** Add `heart: true,` only if you want that city's label to have a heart. Astana already has this.

Saving the object automatically adds its pin, directory entry, city link and tour stop. Counts update automatically. If the country is not already in your country list, it is added to the map as **visited**. An existing home/lived status is preserved.

### A new city in the United States

Also include the **full state name**, which places it correctly in the US state view, including the Alaska and Hawaii insets:

```js
{
  id: "san_francisco",
  name: "San Francisco",
  country: "United States",
  state: "California",
  lat: 37.7749,
  lon: -122.4194,
  story: ``,
  photos: []
},
```

The state is automatically marked as visited. `visitedStates` is for additional states without a city entry. Use full names such as `New Hampshire`, not `NH`.

## 6. Add a country or change its color/status

Add one object inside the `countries: [ ... ]` list:

```js
{ name: "Japan", status: "visited" },
```

The country can be highlighted even if you haven't added any cities there. To change a status, edit just its value:

| Value | Map meaning |
| --- | --- |
| `"home"` | Home-country color, currently Kazakhstan |
| `"lived"` | Lived-in-country color, currently the US and UK |
| `"visited"` | Visited-country color |

Use the exact country names in `COUNTRY-NAMES.md`. Common friendly names such as `United States`, `Czech Republic`, and `Hong Kong` are supported. Hong Kong remains independent of mainland China for travel tracking.

To make a country neutral again, remove its country entry **and** its city entries. Otherwise, a city in that country will automatically mark it visited again.

## 7. Change tour order, rename, or remove a city

The tour follows the order of cities in `content.js` by default. Move entire city objects to change it. To use a separate order, add an optional `tour` list alongside `countries`, `cities`, and `visitedStates`:

```js
tour: ["astana", "almaty", "hong_kong", "london"],
```

Any remaining cities are appended automatically. Tour order does not imply dated flights or actual connecting routes.

Rename a city by changing `name`; keep its `id`. Remove a city by deleting its entire object and its separating comma as needed. You don't have to maintain a second city list, coordinate file or marker definition.

## 8. If something doesn't appear

- **No updated page:** Check the repository's **Actions** tab or **Settings → Pages** for publication status. Wait for the deployment to finish, then hard-refresh.
- **Missing photograph:** Check the path, capitalization, file extension and whether the image was committed. A removed/missing image has an honest unavailable state.
- **Unknown country:** Copy the supported name from `COUNTRY-NAMES.md`.
- **Missing US inset pin:** Include the city's full `state` value.
- **Content error / map won't load:** Check for a missing comma, quote, brace or closing backtick in `content.js`. The page reports readable content-validation errors where possible. On GitHub, open the file's **History** to recover the previous working contents if needed.
- **Tiny or crowded city labels:** Zoom in, or use the searchable directory. Labels stay close to their pins; crowded labels may hide until there is room.

## 9. View mode and ownership

There is no public edit mode, upload form, save button or hidden admin password. Editing takes place in GitHub, which controls who has repository write access. Visitors can explore the globe, read stories, view photos and copy city links. They cannot overwrite the published map.

Editing a page locally in a browser's developer tools affects only that person's browser. It does not change your files or published site.
