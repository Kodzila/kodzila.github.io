# Angular knowledge base

## ChangeDetectionStrategy.OnPush
It is a good practice to change default change detection for Angular components.

If you set up:
```ts
@Component({
    selector: 'isav-content-ownership',
    templateUrl: './content-ownership.html',
    styleUrls: ['./content-ownership.sass'],
    changeDetection: ChangeDetectionStrategy.OnPush,
})
export class IsavContentOwnership implements OnInit
```

Then component will rerender only on change in `Input`, `Output` or `Children`.

If you want to force rerender you need to inject ChangeDetectorRef:
```js
 public constructor(
        private cdRef: ChangeDetectorRef,
    ) {}
```

and manually mark it:
```js
this.cdRef.markForCheck();
```

This approach optimize performance of the application.
Angular team recommends the approach for UI components mostly. Some of people think it should be a hard default.

Probably the reason why it is not hard default is that it's easier to start working with Angular for new people without this approach.
