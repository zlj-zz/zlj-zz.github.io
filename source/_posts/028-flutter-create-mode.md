---
title: Use tools to create mode in Flutter
date: 2020-10-17
change-date: 2020-10-19 11:09
category: Flutter
---

At Flutter app, sometime we need to create `class` for mode. Although it can be done manaually, it takes too much time. So we can use some tools to complete this.

We can use these tools. You can view more usafe methods on the [officail website](pub.dev).

```shell
json_annotation
shared_preferences
json_model
build_runner
json_serializable
```

First, we should add these package to your `pubspec.yaml`, like below. Then run the `pub get` command in terminal to install them. At this point we have made preparations.

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter

  json_annotation: ^2.0.0
  shared_preferences: ^0.5.10

dev_dependencies:
  flutter_test:
    sdk: flutter

  json_model:
    git:
      url: "https://github.com/wbtvc/json_model.git"
  build_runner: ^1.6.0
  json_serializable: ^3.0.0
```

Then we can create `jsons` fold to save our mode json file. The following is a case of caching mode, we can create mulitple json files corresponding to multiple modes.

```json
{
  "enable": true, // 是否启用缓存
  "maxAge": 1000, // 缓存的最长时间，单位（秒）
  "maxCount": 100 // 最大缓存数
}
```

The producted model file:

```dart
/// cacheConfig.dart

import 'package:json_annotation/json_annotation.dart';

part 'cacheConfig.g.dart';

@JsonSerializable()
class CacheConfig {
    CacheConfig();

    bool enable;
    num maxAge;
    num maxCount;

    factory CacheConfig.fromJson(Map<String,dynamic> json) => _$CacheConfigFromJson(json);
    Map<String, dynamic> toJson() => _$CacheConfigToJson(this);
}
```

```dart
/// cacheConfig.g.dart

// GENERATED CODE - DO NOT MODIFY BY HAND

part of 'cacheConfig.dart';

// **************************************************************************
// JsonSerializableGenerator
// **************************************************************************

CacheConfig _$CacheConfigFromJson(Map<String, dynamic> json) {
  return CacheConfig()
    ..enable = json['enable'] as bool
    ..maxAge = json['maxAge'] as num
    ..maxCount = json['maxCount'] as num;
}

Map<String, dynamic> _$CacheConfigToJson(CacheConfig instance) =>
    <String, dynamic>{
      'enable': instance.enable,
      'maxAge': instance.maxAge,
      'maxCount': instance.maxCount,
    };
```

Next we just should use `flutter packages pub run json_model` in terminal and wait a minute, we can see the mode file be producted in `lib/models`. We just include the fold's `index.dart` to use our mode.
