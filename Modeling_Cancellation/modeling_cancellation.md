Modeling Cancellation Risk
================
Eugene Kiepeeh
2026-08-23

``` r
final_hotel_bookings <- read_csv("~/Documents/00 Working Projects/Hotel Data Analytics/Hotel Analytics/Data/Processed_Data/final_hotel_bookings.csv")
```

    ## Rows: 87396 Columns: 37
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (14): hotel, arrival_date_month, meal, country, market_segment, distrib...
    ## dbl  (20): is_canceled, lead_time, arrival_date_year, arrival_date_week_numb...
    ## lgl   (1): booking_changes_flag
    ## date  (2): arrival_date, reservation_status_date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
final_hotel_bookings |> count(hotel)
```

    ## # A tibble: 2 × 2
    ##   hotel            n
    ##   <chr>        <int>
    ## 1 City Hotel   53428
    ## 2 Resort Hotel 33968

``` r
final_hotel_bookings |> group_by(hotel) |> 
  summarise(mean_canceled = mean((is_canceled), na.rm = T), 
            sd_canceled = sd((is_canceled), na.rm = T), .groups = "drop")
```

    ## # A tibble: 2 × 3
    ##   hotel        mean_canceled sd_canceled
    ##   <chr>                <dbl>       <dbl>
    ## 1 City Hotel           0.300       0.458
    ## 2 Resort Hotel         0.235       0.424

``` r
cancel_mod <- glm(is_canceled ~ hotel, data = final_hotel_bookings, family = binomial(link = "logit"))

summary(cancel_mod)
```

    ## 
    ## Call:
    ## glm(formula = is_canceled ~ hotel, family = binomial(link = "logit"), 
    ##     data = final_hotel_bookings)
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)       -0.845463   0.009437  -89.59   <2e-16 ***
    ## hotelResort Hotel -0.335889   0.015903  -21.12   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 102790  on 87395  degrees of freedom
    ## Residual deviance: 102336  on 87394  degrees of freedom
    ## AIC: 102340
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
exp(coef(cancel_mod))
```

    ##       (Intercept) hotelResort Hotel 
    ##         0.4293587         0.7147022

``` r
cancel_mod2 <- glm(is_canceled ~ lead_time, data = final_hotel_bookings, family = binomial(link = "logit"))
summary(cancel_mod2)
```

    ## 
    ## Call:
    ## glm(formula = is_canceled ~ lead_time, family = binomial(link = "logit"), 
    ##     data = final_hotel_bookings)
    ## 
    ## Coefficients:
    ##               Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept) -1.359e+00  1.096e-02 -124.01   <2e-16 ***
    ## lead_time    4.505e-03  8.506e-05   52.96   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 102790  on 87395  degrees of freedom
    ## Residual deviance:  99963  on 87394  degrees of freedom
    ## AIC: 99967
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
exp(coef(cancel_mod2))
```

    ## (Intercept)   lead_time 
    ##   0.2569148   1.0045149

``` r
cancel_mod3 <- glm(is_canceled ~ booking_time_flag, data = final_hotel_bookings, family = binomial(link = "logit"))
summary(cancel_mod3)
```

    ## 
    ## Call:
    ## glm(formula = is_canceled ~ booking_time_flag, family = binomial(link = "logit"), 
    ##     data = final_hotel_bookings)
    ## 
    ## Coefficients:
    ##                        Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)            -2.38533    0.02660  -89.66   <2e-16 ***
    ## booking_time_flag31-90  1.63194    0.03016   54.10   <2e-16 ***
    ## booking_time_flag8-30   1.30620    0.03211   40.68   <2e-16 ***
    ## booking_time_flag90+    1.84653    0.02917   63.30   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 102790  on 87395  degrees of freedom
    ## Residual deviance:  97109  on 87392  degrees of freedom
    ## AIC: 97117
    ## 
    ## Number of Fisher Scoring iterations: 5

``` r
exp(coef(cancel_mod3))
```

    ##            (Intercept) booking_time_flag31-90  booking_time_flag8-30 
    ##             0.09205895             5.11379764             3.69212784 
    ##   booking_time_flag90+ 
    ##             6.33780945

``` r
# Cancellation Probabilities by Booking TIme
table(final_hotel_bookings$booking_time_flag,
        as.character(final_hotel_bookings$is_canceled)) |> proportions()
```

    ##        
    ##                  0          1
    ##   0-7   0.19178223 0.01765527
    ##   31-90 0.17694174 0.08329901
    ##   8-30  0.13953728 0.04742780
    ##   90+   0.21684059 0.12651609

``` r
adr_data <- final_hotel_bookings |> filter(adr == 0)
nights_data <- final_hotel_bookings |> filter(total_nights < 1)
```
