---
title: ""
emoji: "📀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["java", "react"]
published: false
---
はじめに
Java で月を任意の表示形式に変換する
<https://www.perplexity.ai/search/anataha-java-nohurohuetusiyona-Z7m97ltYRl6t6o67cr_AMw>

## 内容

```java
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeFormatterBuilder;
import java.time.temporal.ChronoField;
import java.util.HashMap;
import java.util.Locale;
import java.util.Map;

public class CustomDateFormatter {
    private static final DateTimeFormatter standardFormatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
    private static final DateTimeFormatter customFormatter;

    static {
        Map<Long, String> monthMap = new HashMap<>();
        monthMap.put(1L, "睦月"); monthMap.put(2L, "如月"); monthMap.put(3L, "弥生");
        monthMap.put(4L, "卯月"); monthMap.put(5L, "皐月"); monthMap.put(6L, "水無月");
        monthMap.put(7L, "文月"); monthMap.put(8L, "葉月"); monthMap.put(9L, "長月");
        monthMap.put(10L, "神無月"); monthMap.put(11L, "霜月"); monthMap.put(12L, "師走");

        customFormatter = new DateTimeFormatterBuilder()
            .appendPattern("yyyy年")
            .appendText(ChronoField.MONTH_OF_YEAR, monthMap)
            .appendPattern("月d日")
            .toFormatter(Locale.JAPANESE);
    }

    public static String formatDate(LocalDate date, String pattern) {
        if ("yyyy年M月d日".equals(pattern)) {
            return date.format(customFormatter);
        } else {
            return date.format(standardFormatter);
        }
    }

    public static void main(String[] args) {
        LocalDate date = LocalDate.of(2024, 7, 3);
        System.out.println(formatDate(date, "yyyy年M月d日")); // 出力: 2024年文月3日
        System.out.println(formatDate(date, "yyyy/MM/dd")); // 出力: 2024/07/03
    }
}
```

## まとめ
