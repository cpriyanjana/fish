# fish
library(ggplot2)
library(dplyr)
library(tidyverse)
library(readxl)
library(here)
library(kableExtra)
library(stringr)

rm(list = ls())

fish <- read_csv("fish.csv")
kelp_abur <- read_excel("kelp_fronds.xlsx", sheet = "abur")

fish_garibaldi <- fish %>% filter(common_name == "garibaldi")
fish_mohk <- fish %>% filter(site == "mohk")

## Filter by numeric conditions
fish_over50 <- fish %>% filter(total_count >= 50)

## Filter by multiple conditions
fish_3sp <- fish %>% filter(common_name %in% c("garibaldi", "blacksmith", "black surfperch"))
fish_gar_2016 <- fish %>% filter(year == 2016 | common_name == "garibaldi")

## Filter by multiple conditions
aque_2018 <- fish %>% filter(year == 2018 & site == "aque")

## Additional filters
low_gb_wr <- fish %>% filter(common_name %in% c("garibaldi", "rock wrasse"), total_count <= 10)
fish_bl <- fish %>% filter(str_detect(common_name, "black"))
fish_it <- fish %>% filter(str_detect(common_name, "it"))  # Matches "blacksmITh" and "senorITa"

## Full Join
abur_kelp_fish <- kelp_abur %>% full_join(fish, by = c("year", "site"))

## Left Join
kelp_fish_left <- kelp_abur %>% left_join(fish, by = c("year", "site"))

## Inner Join
kelp_fish_injoin <- kelp_abur %>% inner_join(fish, by = c("year", "site"))

## Filter and Join in Sequence
my_fish_join <- fish %>% 
  filter(year == 2017, site == "abur") %>% 
  left_join(kelp_abur, by = c("year", "site")) %>% 
  mutate(fish_per_frond = total_count / total_fronds)

# Creating an HTML Table
my_fish_join %>%
  kable() %>%
  kable_styling(bootstrap_options = "striped", full_width = FALSE)

![image](https://github.com/user-attachments/assets/aae85bde-074f-4f4e-b95e-f52b4f2e02d9)
