# RxJs knowledge base

## Active URL segments
```ts
private routeSub = Subscription.EMPTY;

constructor(
    private router: Router,
    private location: Location,
    private elementRef: ElementRef
) {}

ngOnInit() {
    this.routeSub = this.router.events
        .pipe(
            filter((event) => event instanceof NavigationEnd),
            map(() => this.location.path()),
            startWith(this.location.path()),
            map((pathname) => pathname.split('/').indexOf(this.activeUrlSegment) !== -1)
        )
        .subscribe((match) => {
            const classList = this.elementRef.nativeElement.classList;
            if (match) {
                classList.add(this.activeClass);
            } else {
                classList.remove(this.activeClass);
            }
        });
}
```

## Debounce functions (multiple subsequent calls, only one propagation)
```ts
export function debounceTick<T>(): (source: Observable<T>) => Observable<T> {
    return (source) => {
        return new Observable<T>((subscriber) => {
            let isCompleted = false;
            let timeout: null | number = null;
            let lastValue: T;

            const sub = source.subscribe({
                next: (value) => {
                    lastValue = value;

                    if (!timeout) {
                        timeout = setTimeout(() => {
                            subscriber.next(lastValue);
                            if (isCompleted) subscriber.complete();
                            timeout = null;
                        });
                    }
                },
                complete: () => {
                    isCompleted = true;
                },
                error: (err) => subscriber.error(err),
            });

            return () => {
                sub.unsubscribe();
                if (timeout) clearTimeout(timeout);
            };
        });
    };
}
```
Test:
```ts
describe('debounceTick()', function () {
    it('should emit 3 on next tick', fakeAsync(function () {
        const cb = jest.fn();
        of(1, 2, 3).pipe(debounceTick()).subscribe(cb);
        tick();
        expect(cb).toHaveBeenCalledTimes(1);
        expect(cb).toHaveBeenCalledWith(3);
    }));
});
```

Usage:
```ts
search$ = createEffect(() => {
        return this.actions.pipe(
            ofType(beginSearch),
            withLatestFrom(this.store.select(selectGlobalSearch)),
            debounceTick(),
            mergeMap(([action, search]) => {
                //...
            })
        );
    });
```
