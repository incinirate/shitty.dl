# shitty.download

The backend behind shitty.download.

## Configuration

Make a file called `config.json`, with the following properties:

```json
{
    "logo": "poop.png",
    "name": "shitty.download",
    "name_color": "#a5673f",
    "title": "lemmmy's file host lies here",
    "disclaimer": "for dmca etc., contact drew at lemmmy dot pw",
    "password": "",
    "imagePath": "/path/to/images",
    "url": "https://your.host/",
    "listen": "3000",
    "fileLength": 4,
    "pasteThemePath": "https://atom.github.io/highlights/examples/atom-dark.css",
    "oldPasteThemeCompatibility": true,
    "sessionSecret": "",
	"uploadDeleteLink": true,
    "languagePackages": [
      "language-lua"
    ]
}
```
- `logo` - Filename of your logo image. Make sure to put this in /public.
- `name` - Name of your file host.
- `name_color` - Color of your file host's name.
- `title` - Title on the homepage.
- `disclaimer` - Message used to supply important info about takedowns/dmcas, or a disclaimer.
- `password` - The sha256 hash of your password. Calculate with `echo -n 'password' | sha256sum`
- `imagePath` - The absolute path to your image storage directory.
- `url` - The full URL of your host to be prepended to image paths. Trailing slash required.
- `listen` The port or unix sock to listen to. Can also be options object as [defined here](https://nodejs.org/api/net.html#net_server_listen_options_callback)
- `fileLength` Amount of characters in generated filenames. Default is 4.
- `pasteThemePath` Location of a theme for the paste syntax highlighter.
- `oldPasteThemeCompatibility` Mode that enables compatibility with older Atom themes. **NOTE: For the time being, you must run `sed -i "s/atom-text-editor/.editor/g" stylesheet.css` if you aren't using an older atom theme**
- `sessionSecret` Secret for session storage. Should be a secure random string.
- `uploadDeleteLink` If set to `true`, files uploaded via the API will contain a deletion link. Else, the password or logging in will be required to delete files.
- `languagePackages` Array of npm package names containing atom language grammars.

## Custom names

You can implement your own custom naming function by creating a file called `custom-name.js` that exports a function that returns a filename. Here is an example:

```js
const _ = require("lodash");
const fs = require("fs");

const nouns = fs.readFileSync("nouns.txt").toString().split("\n");

module.exports = () => {
	const noun = _.sample(nouns);
	return (noun.match(/^[aeiou]/i) ? "an-" : "a-") + noun;
};
```

More examples for custom naming functions can be found in the [examples directory](https://github.com/Lemmmy/shitty.dl/tree/master/examples).

## Notes

* Ensure that the user you run the shitty.dl process as has write permissions to `imagePath`.
* Make sure your paste theme has the same scheme as your site - i.e. if your site is HTTPS, the theme must be HTTPS too.
