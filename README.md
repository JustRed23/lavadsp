[![](https://jitpack.io/v/JustRed23/lavadsp.svg)](https://jitpack.io/#JustRed23/lavadsp)

# lavadsp

A bunch of lavaplayer audio filters implemented with native code

## Getting Started

### Installing

Replace `VERSION` with the version you want to use. The latest version can be found in the badge above.

#### Maven

```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.github.JustRed23</groupId>
        <artifactId>lavadsp</artifactId>
        <version>VERSION</version>
    </dependency>
</dependencies>
```

#### Gradle

```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    compile 'com.github.JustRed23:lavadsp:VERSION'
}
```

### Basic Usage

```java
AudioPlayer player = manager.createPlayer();
player.setFilterFactory((track, format, output)->{
    TimescalePcmAudioFilter audioFilter = new TimescalePcmAudioFilter(output, format.channelCount, format.sampleRate);
    audioFilter.setSpeed(1.5); //1.5x normal speed
    return Collections.singletonList(audioFilter);
});
```

### Chaining

Filters may be chained to merge their effects:
```java
AudioPlayer player = manager.createPlayer();
player.setFilterFactory((track, format, output)->{
    TremoloPcmAudioFilter tremolo = new TremoloPcmAudioFilter(output, format.channelCount, format.sampleRate);
    tremolo.setDepth(0.75);
    TimescalePcmAudioFilter timescale = new TimescalePcmAudioFilter(tremolo, format.channelCount, format.sampleRate);
    timescale.setSpeed(1.5);
    return Arrays.asList(timescale, tremolo);
});
```

To dynamically choose which filters to use, you can see [this example](https://github.com/natanbc/andesite/blob/0ee816125c99ca0921a1cb4280f30398ac520a9d/api/src/main/java/andesite/player/filter/FilterChainConfiguration.java#L197-L218)
