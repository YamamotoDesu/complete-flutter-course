## Helper method
Take-away: creating helper methods is an anti-pattern that can hurt the performance of your Flutter apps and make your widgets less reusable.

This is best explained by this great video by the Flutter team:

[Widgets vs helper methods | Decoding Flutter](https://www.youtube.com/watch?v=IOyq-eTRhvo)

## Resources
Since Flutter 2.5, we can use the new skeleton app template when creating a new project (flutter create -t skeleton), and this will generated all the required setup for us.

## Transition
### Default
<img width="300" alt="image" src="https://github.com/YamamotoDesu/complete-flutter-course/assets/47273077/b4af7c61-46f6-43ab-9e5e-da36b28dd48b">

```dart
GoRoute(
   path: 'cart',
   builder: (context, state) => const ShoppingCartScreen(),
```

### Modal
<img width="300" alt="image" src="https://github.com/YamamotoDesu/complete-flutter-course/assets/47273077/e4355527-9494-4fb3-bf36-52f6cb6beb09">

```dart
GoRoute(
   path: 'cart',
   pageBuilder: (context, state) => MaterialPage(
   key: state.pageKey,
   fullscreenDialog: true,
   child: const ShoppingCartScreen(),
```

## Repository Pattern

![image](https://github.com/YamamotoDesu/complete-flutter-course/assets/47273077/d9864c38-5fc4-4973-aa29-4de485b2e4ef)
