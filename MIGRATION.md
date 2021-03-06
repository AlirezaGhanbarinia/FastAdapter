###Upgrade Notes

#### v2.5.3
* the `ItemTouchCallback` has a new method `itemTouchDropped` just implement it and keep it empty, if you do not need it.

#### v2.5.0
**WARNING**
* If you use the MaterialDrawer or the AboutLibraries you will need a compatible release of those for it to work
* The FACTORY within the items is no longer required and replaced by a much simpler approach. Remove the old FACTORY and it's methods, and implement the `getViewHolder` method
```
@Override
public ViewHolder getViewHolder(View v) {
    return new ViewHolder(v);
}
```
* If you implemented your own `Item` using the `IItem` interface instead the `AbstractItem` you will now also have to implement
  * `attachToWindow` and
  * `detachFromWindow`
* The reflection based `GenericItemAdapter` CTOR (with the 2 classes as overload) was removed. Please use the CTOR using a `Function`

#### v2.1.0 
* This release brings minor changes to the `IItem` interface. Please update MaterialDrawer and AboutLibraries too if you use them.
* Starting with this version there is now the `core` and the `commons` dependency. Which make the `FastAdapter` even slimmer if you want to use the basics. 

**QUICK UPGRADE**
* The ` List payloads` param in the `onBindViewHolder` method was changed to `List<Object> payloads`
* The FastAdapter is now split into smaller dependencies
 * `com.mikepenz:fastadapter` : just contains the basics (no `FastItemAdapter` for example)
 * `com.mikepenz:fastadapter-commons` : this one contains the `FastItemAdapter`  and more useful common helper classes (please make sure to update the imports as these classes moved to `com.mikepenz.fastadapter.commons.*`)
 * `com.mikepenz:fastadapter-extensions` : comes with additional utils and helpers which bring allow more complex and advanced implementations

#### v2.0.0 
* This release brings new interfaces, and many changes to the expandable behavior. There is now also a `unbindView` method you have to implement for your `IItem`s.

**SHORT OVERVIEW**
* If you have items implemented by using the interface you have to implement the new methods (**unbindView**)
* If you have expandable items make sure to adjust the generic type definitions as metioned below. Check out the `AbstractExpandableItem` to simplify this for you
* If you use the `MaterialDrawer`, the `AboutLibraries` in your project, please make sure to update them so the changed interfaces do not cause conflicts

**DETAILED**
* New `unbindView` method was added to the `IItem` --> This method is called when the current item is no longer set and before the `ViewHolder` is used for the next item
 * You should move your view resetting logic here, or for example glide image loading canceling
* `IExpandable` generic types changes
 * it is now required to define the type which will be used for the subItems. This can be an implementation or `ISubItem`. We now require this, as we will keep the references between childs, and parents.
 * this allows more optimizations, and many additional usecases
* New `ISubItem` interface added 
 * items serving as subitems, have to implement this. 
* New `AbstractExpandableItem` added, which combines `IExpandable` and `ISubItem` with an `AbstractItem` to simplify your life
* A new `SubItemUtil` was introduced which simplifies some use cases when working with expandable / collapsing lists

**Extensions**
* new `EndlessRecyclerOnTopScrollListener` available
* new `IExtendedDraggable` and `DragDropUtil` introduced
* new `RangeSelectorHelper` introduced

#### v1.8.0
* This release bring a breaking interface change. Your items now have to implement `bindView(ViewHolder holder, List payloads)` instead of `bindView(VH holder)`. 
 * The additional payload can be used to implement a more performant view updating when only parts of the item have changed. Please also refer to the `DiffUtils` which may provide the payload.

#### v1.7.0
* **Dropping support for API < 14. New `MinSdkVersion` is 14**

#### v1.5.8 -> 1.6.0 
* the `IExpandable` interface has a new method `isAutoExpanding` which needs to be implemented (default value `true`). This allows to disable the auto toggling of `Expandable` items in the `FastAdapter` which is a problem for custom behaviors. Like issue #157

#### v1.x.x -> v1.4.0
* with v1.4.0 by default a FastAdapter is now `withSelectable(false)` (for normal lists) if you have selection enabled in your list, add `withSelectable(true)` to your `FastAdapter`, `FastItemAdapter` or `GenericFastItemAdapter``
