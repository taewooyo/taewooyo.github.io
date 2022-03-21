---
layout: post
title:  "recyclerview 효율성(DiffUtil, AsyncListDiffer)"
categories: Android
author: bn-tw2020
---
* content
  {:toc}


## recyclerView

- 안드로이드에서 동일한 레이아웃을 반복적으로 뿌려주는 리스트형 UI를 만들기 위해 recyclerView가 사용된다.

  Adapter, LayoutManager, ViewHolder 3가지를 이용한다.

- Adapter는 데이터 리스트를 실제로 눈으로 볼 수 있게 itemview로 변환해주는 매개체 역할을 한다.

> Adapter 역할

- recyclerview에 보여줄 데이터 리스트 관리

- view 객체를 재사용하기 위한 ViewHolder 객체 생성

- 데이터 리스트에서 position에 해당하는 데이터를 itemview에 표시





## 문제점

- recyclerview에서 데이터 리스트를 갱신할 때, `notifyDataSetChanged()` 호출한다.

  Adapter에게 recyclerview의 데이터 리스트가 바뀌었으니 모든 항목을 다시 업데이트하라고 메시지를 보낸다.

  100,000개 데이터를 가지고 있는 리스트에서 1개의 데이터만 바뀌면, `notifyItemChanged(position)` 으로 성능을 올려줄 수 있다.

  위치가 무작위라면 어느 곳인지 알 수가 없게된다. 그래서 실질적으로 100,000개를 갱신해줘야한다.

- 불필요한 교체 비용을 줄이기 위해 `DiffUtil` 을 이용한다.


## DiffUtil

- recyclerview의 성능을 개선하기 위해 해주는 유틸리티 클래스이다.

  기존 데이터 리스트와 교체할 데이터 리스트를 비교해서 필요한 데이터들을 추려낸다.

> 사용법

- 2개의 리스트를 비교하기 위해 DiffUtil.Callback() 을 구현해야 한다.

- areItemsTheSame() : 두 아이템이 동일한 아이템인지 확인(고유한 ID)

- areContentsTheSame() : 두 아이템이 동일한 내용물을 가지고 있는지 확인 / areItemsTheSame()이 true 이어야한다.

```kotlin
class BaseDiffUtil<T>(private val newList: List<T>, private val oldList: List<T>) : DiffUtil.Callback() {

    override fun getOldListSize(): Int = oldList.size

    override fun getNewListSize(): Int = newList.size

    override fun areItemsTheSame(oldItemPosition: Int, newItemPosition: Int) =
        newList[newItemPosition] == oldList[oldItemPosition]

    override fun areContentsTheSame(oldItemPosition: Int, newItemPosition: Int) =
        newList[newItemPosition] == oldList[oldItemPosition]
}
```

- DiffUtil.calculateDiff(diffUtil)로 업데이트가 필요한 리스트를 찾는다.

  notifyDataSetChanged() 대신 dispatchUpdatesTo(adapter: Adapter)를 사용해서

  교체가 필요한 아이템에 대해서 부분적으로 교체한다.

```kotlin
abstract class BaseRecyclerView<B : ViewDataBinding, T : Any>(
    @LayoutRes private val layoutResId: Int
) : RecyclerView.Adapter<BaseViewHolder<B, T>>() {
    protected val items = mutableListOf<T>()

    fun setItems(newItems: List<T>) {
        val diffUtil = BaseDiffUtil(items, newItems)
        val diffResult = DiffUtil.calculateDiff(diffUtil)

        items.clear()
        items.addAll(newItems)
        diffResult.dispatchUpdatesTo(this)
    }

  // ...
}
```

- 아이템 개수가 너무 많아지면 비교 연산 시간이 길어진다.

  그런 경우, calculateDiff는 백그라운드 쓰레드에서 처리를 한다.

- 처리 할 수 있는 리스트의 최대 크기는 2^26 정도 된다.


## AsyncListDiffer

- DiffUtil을 보다 쉽게 사용할 수 있게 해주는 클래스이다.

  자체적으로 멀티 쓰레드에 대한 처리가 되어있기 때문에 개발자가 직접 동기화 처리를 할 필요가 없어진다.

- AsyncDifferConfig로 backgroundThreadExecutor를 따로 설정하지 않으면,

  default로 Executors.newFixedThreadPool(2)로 쓰레드 풀을 하나 만들어서 비교 연산한다.

> 사용법

- 아이템을 비교할 때 호출할 DiffUtil.ItemCallback을 구현한다.

```kotlin
class PhotosDiffUtilCallback : DiffUtil.ItemCallback<Photo>() {

    override fun areItemsTheSame(oldItem: Photo, newItem: Photo) =
        oldItem.photo.id == newItem.photo.id

    override fun areContentsTheSame(oldItem: Photo, newItem: Photo) =
        oldItem.photo == newItem.photo
}
```

- Adapter 내부에 AsyncListDiffer 객체를 선언해서 사용한다.

  getCurrentList() : adapter에서 사용하는 item 리스트에 접근할 때 사용한다.

  submitList(List<T> newList) : 리스트 데이터를 교체할 때 사용한다.

```kotlin
class PhotosRecyclerAdapter : RecyclerView.Adapter<PhotoViewHolder>() {
    private val diffUtil = AsyncListDiffer(this, PhotosDiffUtilCallback())

    fun replaceTo(newItems: List<Photo>) = diffUtil.submitList(newItems)

    fun getItem(position: Int) = diffUtil.currentList[position]

    override fun getItemCount(): Int = diffUtil.currentList.size

    override fun onBindViewHolder(holder: PlaceViewHolder, position: Int) =
        holder.bind(getItem(position))

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int) = PhotoViewHolder(
        ItemPlaceBinding.inflate(LayoutInflater.from(parent.context), parent, false)
    )
}
```

## ListAdapter

- AsyncListDiffer를 좀더 간편하게 사용할 수 있게 해준다.

  AsyncListDiffer에서 교체하기 위한 `replaceTo()`, 특정 위치를 반환하는 `getItem()` 등의

  손수 개발해야 하는 기능들 할 필요 없게 해준다.

- ListAdapter는 AsyncListDiffer의 wrapper 클래스이다.

  RecyclerView.Adapter<VH>를 구현하고 있다.

> 사용법

- getCurrentList() : 현재 리스트 반환

  onCurrentListChanged() : 리스트가 업데이트 되었을 때 실행할 콜백 지정

  submitList(MutableList<T> list) : 리스트 데이터를 교체 할 때 사용

- `val photoAdapter = PhotosRecyclerAdapter()`

  `photoAdapter.submitList(newItems)` 로 부분적으로 수정한다.

```kotlin
class PhotosRecyclerAdapter : ListAdapter<Photo, PhotoViewHolder>(PhotosDiffUtilCallback()) {

    override fun onBindViewHolder(holder: PhotoViewHolder, position: Int) =
        holder.bind(getItem(position))

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int) = PhotoViewHolder(
        ItemPhotoBinding.inflate(LayoutInflater.from(parent.context), parent, false)
    )
}
```

## Reference

- [설명](https://medium.com/@jungil.han/recyclerview-%EA%B0%9C%EB%B0%9C%EC%97%90-%EB%82%A0%EA%B0%9C-%EB%8B%AC%EA%B8%B0-539e08291160)

- [예제](https://voiddani.tistory.com/7)

- [queue을 이용한 리스트 관리](https://jonfhancock.com/get-threading-right-with-diffutil-423378e126d2)

- [Android Developers ListAdapter](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/ListAdapter)

- [Android Developers DiffUtil](https://developer.android.com/reference/androidx/recyclerview/widget/DiffUtil)