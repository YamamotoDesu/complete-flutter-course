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

## Reading providers with ConsumerWidget and Consumer
Before:
```dart
class ProductsGrid extends ConsumerWidget {
 
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final productsRepository = ref.watch(productsRepositoryProvider);
    final products = productsRepository.getProductsList();
    return SomeWidget(products);
  }
}
```

Instead, if we have a StatefulWidget, we should convert it to ConsumerStatefulWidget as explained here.

Alternatively, we can use Consumer:
```dart
class ProductScreen extends StatelessWidget {
  const ProductScreen({Key? key, required this.productId}) : super(key: key);
  final String productId;
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: const HomeAppBar(),
      body: Consumer(
        builder: (context, ref, _) {
          final productsRepository = ref.watch(productsRepositoryProvider);
          final product = productsRepository.getProduct(productId);
          return SomeWidget(product);
        },
      ),
    );
  }
}
```

The Consumer child argument
The Consumer widget gives us a third child argument that is used for performance optimization. Read more here.
https://pub.dev/documentation/flutter_riverpod/latest/flutter_riverpod/Consumer-class.html
![image](https://github.com/YamamotoDesu/complete-flutter-course/assets/47273077/b1038c79-d8c3-489f-820f-dd9f1da20d2f)

## StreamProvider

```dart
class ProductsGrid extends ConsumerWidget {
  const ProductsGrid({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final productsListValue = ref.watch(productsListProvider);
    return productsListValue.when(
      data: (products) => products.isEmpty
          ? Center(
              child: Text(
                'No products found'.hardcoded,
                style: Theme.of(context).textTheme.headlineMedium,
              ),
            )
          : ProductsLayoutGrid(
              itemCount: products.length,
              itemBuilder: (_, index) {
                final product = products[index];
                return ProductCard(
                  product: product,
                  onPressed: () => context.goNamed(
                    AppRoute.product.name,
                    pathParameters: {'id': product.id},
                  ),
                );
              },
            ),
      error: (e, st) => Center(child: ErrorMessageWidget(e.toString())),
      loading: () => const CircularProgressIndicator(),
    );
  }
}

```

## FutureProvider

