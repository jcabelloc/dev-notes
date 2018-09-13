# Angular Material Notes

### Set Up

* Install Angular Material and Angular CDK
```bash
npm install --save @angular/material @angular/cdk

```

* Animations
```bash
npm install --save @angular/animations
```
```ts
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';

@NgModule({
  ...
  imports: [BrowserAnimationsModule],
  ...
})
export class AppModule { }`
```

* Import the component modules
```ts
import {MatButtonModule, MatCheckboxModule} from '@angular/material';

@NgModule({
  ...
  imports: [MatButtonModule, MatCheckboxModule],
  ...
})
export class AppModule { }
```

* Include a theme (Global styles.css)

```css
@import "~@angular/material/prebuilt-themes/indigo-pink.css";
```

* Add Material Icons ( load the icon font in index.html)
```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```