# Diff-Between-Flow-and-LiveData

What are the special reasons to use Flow instead of LiveData ?

LiveData Library is a part of Android Framework. Flow is a part of Kotlin Language.

When we are building KMM(Kotlin Multiplatform Mobile) applications for both Android and IOS , it is required to keep as much as possible codes in pure Kotlin, so that we can share them betwceen both platform specific code bases.

If we use Flow, we will be able to use repository and other data source classes(data layer) written in Kotlin as a part of the code base of IOS app.

Here are the steps to use Flow instead of LiveData in our project.

1) Change getSubscribers() function in SubscriberDAO.kt.  Also don't forget to   import kotlinx.coroutines.flow.Flow

@Query("SELECT * FROM subscriber_data_table")
fun getAllSubscribers(): Flow<List<Subscriber>>

2) Add this function to SubscriberViewModel.kt. Inside this function we are converting flow to a LiveData.

fun getSavedSubscribers() = liveData {
    repository.subscribers.collect {
        emit(it)
    }
}

3) Inside MainActivity.kt, change the displaySubscriberrsList() function to Observe above LiveData.

  private fun displaySubscribersList() {
    subscriberViewModel.getSavedSubscribers().observe(this, Observer {
        adapter.setList(it)
        adapter.notifyDataSetChanged()
    })
}
