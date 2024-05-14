﻿# CMSC320-Final-Tutorial
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "By: Isabella Battish, Janet Choi, Sarah Hwang\n",
        "\n",
        "\n",
        "https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DFB6NG\n",
        "\n",
        "\n",
        "Tutorial To-Do List\n",
        "1. Discuss our research question and goals\n",
        "2. Discuss why our topic is important\n",
        "3. 'Literature review' (we need to do some research and link it)\n",
        "4. Data Processing- deleting unnecessary columns and such\n",
        "5. Exploratory data analysis- get some descriptive statistics and graphs\n",
        "6. Machine learning- look at multiple models (?)\n",
        "7. write up on results and findings\n",
        "8. add to GitHub pages\n",
        "9. Tidy up and make pretty"
      ],
      "metadata": {
        "id": "K-vJ-Y_t4Laa"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "(Discussion of why our topic is important and research questions/goals and literature review)\n",
        "\n",
        "\n",
        "Drinking water infrastructure is critical to a safe and healthy population. Every living being needs safe and clean drinking water, in fact, it's one of the [UN's Sustainable Development goals](https://sdgs.un.org/goals). While, the U.S. does have better drinking water quality and infrastructure compared to many places, the quality of drinking water is not equal everywhere in the U.S. The U.S. has old infrastructure and repairs and maintenance are lagging, especially in places with limited public spending. The older pipes can corrode and contaminate drinking water with materials we don't want to drink such as lead. The most famous recent (and ongoing) case of this is in Flint, Michigan where city employees tried to cut costs on water allowing 'hard' (mineral dense) water from the Flint River enter old pipes and corrode them so that they contaminated the drinking water with lead. Another famous example is of water that was capable of catching on fire in West Virginia after a chemical that is used to clean coal was spilled. Other places that have had issues with water include Jackson, Mississippe, NYC, Baltimore, MD, and others. These cases show just how critical it is that we ensure our water quality and the infrastructure that brings us our water is up to date and properly functioning.\n",
        "\n",
        "In this tutorial, we will walk you through the Data Science Pipeline.\n",
        "1. Data Collection\n",
        "2. Data Processing\n",
        "3. Exploratory Analysis and Data Visualization\n",
        "4. Anaylysis and Hypothesis Testing and Machine Learning\n",
        "5. Insight and Policy Decision.\n",
        "\n",
        "We hope that this walkthrough will show insights on the U.S. drinking water infrastructure, while also shedding light on potential risk areas in drinking water.\n",
        "\n",
        "Throughout this tutorial we will analyze a variety of factors, and how they pertain to drinking water. (MORE ABOUT VARIABLES WE'LL ANALYZE HERE).\n",
        "\n",
        "To learn more about drinking water crises in the U.S. please visit these links:\n",
        "- https://www.pbs.org/newshour/show/why-american-cities-are-struggling-to-supply-safe-drinking-water\n",
        "- https://www.npr.org/sections/thetwo-way/2016/04/20/465545378/lead-laced-water-in-flint-a-step-by-step-look-at-the-makings-of-a-crisis\n",
        "- https://www.theatlantic.com/national/archive/2014/01/watch-west-virginia-man-set-tap-water-fire-after-local-chemical-spill/356920/\n",
        "\n",
        "Picking out variables I think may be fun to use, looking at the tutorials we may want to do like 5-7 or smth\n",
        "- Democrat vote share in 2016\n",
        "- source of drinking water\n",
        "- Health Expenditures\n",
        "- Park & Rec Expenditures\n",
        "- Water Utility Construction Costs\n",
        "- Water Utility Total Costs\n",
        "- Water Utility Revenue\n",
        "- Taxes in an area\n",
        "- a place's moisture index\n",
        "- number of full time employees in a municipality\n",
        "- total number of health violations\n",
        "- median income\n",
        "- population\n",
        "- Black population\n",
        "- state & year"
      ],
      "metadata": {
        "id": "Ie6WSX9gk7Jk"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "The first step in the Data Science pipeline is **Data Collection**. Data Collection can mean scraping data from a website using the Python BeautifulSoup library, using http requests to get data from a website, or using Pandas to read in a csv file into a data frame. In this project we will be reading in a csv file. The data in the csv file can be found at https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DFB6NG. The dataset is also called the Municipal Drinking Water Database (MDWD) and uses data from the Census to help researchers interested in learning more about drinking water and factors that influence it. All we have to do is import all of our necessary imports for the tutorial, and run pd.read_csv. We call df.head() to show that we were successful in collecting our data."
      ],
      "metadata": {
        "id": "cUNV6wXJs3MO"
      }
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "lHCoE6Zs2t7R"
      },
      "outputs": [],
      "source": [
        "import pandas as pd\n",
        "import seaborn as sns"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df= pd.read_csv('MDWD.csv', encoding= 'latin-l')\n",
        "df.head()"
      ],
      "metadata": {
        "id": "fVrJAj8A4ArK"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "The second step in the Data Science pipeline is **Data Processing**. In the data processing stage we 'clean' our data so it is an easy to understand and workable format. Our data has A LOT of columns, and we will not be using the data in all of those columns. To get rid of many of the columns we drop/delete all of the columns where financial expenses are 'nonadjusted'. The nonadjusted part means that it's the cost of something but not changed to reflect inflation. We only want the adjusted expenditures. Now our data is a little bit easier to read.\n",
        "\n",
        "**RUN THIS BLOCK ONCE**"
      ],
      "metadata": {
        "id": "XZRViBgGOm_g"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df= df.drop([\"Police_Prot_Total_Exp_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Water_Util_Current_Exp_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Water_Util_Construct_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Water_Util_Inter_Exp_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Health_Total_Expend_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Parks___Rec_Total_Exp_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Total_Debt_Outstanding_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Total_IG_Revenue_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Total_Rev_Own_Sources_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Total_Revenue_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Total_Expenditure_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Total_Taxes_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Water_Utility_Revenue_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Water_Util_Total_Exp_nonadjusted\"], axis= 1)\n",
        "df= df.drop([\"Water_Util_Cap_Outlay_nonadjusted\"], axis= 1)\n",
        "\n",
        "#Some other columns I don't anticipate us needing\n",
        "df= df.drop([\"Police_Prot_Total_Exp\"], axis= 1)\n",
        "df= df.drop([\"GOVSid\"], axis= 1)\n",
        "df= df.drop([\"Total_IG_Revenue\"], axis= 1)\n",
        "df= df.drop([\"Total_Rev_Own_Sources\"], axis= 1)\n",
        "df= df.drop(['FIPS_2000'], axis= 1)\n",
        "df= df.drop(['FIPS_2010'], axis= 1)\n",
        "df= df.drop(['FIPS_Code_State'], axis= 1)\n",
        "df= df.drop(['FIPS_County'], axis= 1)\n",
        "df= df.drop(['FIPS'], axis= 1)\n",
        "df= df.drop(['FIPS_Place'], axis= 1)\n",
        "df= df.drop(['FIPS_Combined'], axis= 1)\n",
        "\n",
        "df.head()"
      ],
      "metadata": {
        "id": "mQbYvTP1OlPQ"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "We have A LOT of NaNs/missing values in our dataset and we have to address them. We noticed that the last column, demshare_pres_2016 had NaNs in every year but 2016. So we create a sub dataframe called df_2016, which only has values from 2016 and we delete demshare_pres_2016 from our main dataframe.\n",
        "\n",
        "**RUN THIS BLOCK ONCE**"
      ],
      "metadata": {
        "id": "0DZtEgCwGae_"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df_2016= df.loc[df['YEAR'] == 2016]\n",
        "df= df.drop(['demshare_pres_2016'], axis= 1)\n",
        "df_2016.head()"
      ],
      "metadata": {
        "id": "F5RmEHXPJ06v"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "The third step in the Data Science Pipeline is **Exploratory Data Analysis and Data Visualization**. In this step we start to explore basic understandings of our data and any trends, patterns, and/or relationships in it. For our data visualization component, we will be using the Seaborn Python library.\n",
        "\n",
        "For starters, we are curious if there's a relationship between where a municipalities drinking water is sourced and what percentage of the area voted for Hillary Clinton in the 2016 election."
      ],
      "metadata": {
        "id": "mHvKy7gXM4bS"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "sns.violinplot(x='PRIMARY_SOURCE_CODE', y='demshare_pres_2016',\n",
        "             data=df_2016)"
      ],
      "metadata": {
        "id": "3fcoMKhTM3jz"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "Interesting, our results don't show any relationship between the source of the water and the percentage of votes for the Democratic presidential candidate in 2016. For more context on what the source codes mean\n",
        "- SWP= Purchased Surface Water\n",
        "- SW= Surface Water\n",
        "- GW= Ground Water\n",
        "- GWP= Purchased Ground Water\n",
        "- GU= Ground Water Influenced by Surface Water\n",
        "- GUP= Purchased Ground Water Influenced by Surface Water\n",
        "\n",
        "Ground water influenced by surface water means that the source of the water is groundwater but it's close enough to the surface to be influenced by surface water recharge. This can lead to contaminants that aren't typically found in groundwater could be found in GU or GUP. To learn more about GU follow this [link](https://oehs.wvdhhr.org/eed/source-water-assessment-wellhead-protection/groundwater-under-direct-influence-of-surface-water-gwudi/).\n",
        "\n",
        "There is also only one observation in 2016 that used GUP, let's explore that further. Based on the codeblock below, it looks like South Sioux City, Nebraska is the municipality with purchased Ground Water Influenced by Surface Water.\n"
      ],
      "metadata": {
        "id": "MBdUL_maRDD3"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df_2016.loc[df_2016['PRIMARY_SOURCE_CODE']=='GUP']"
      ],
      "metadata": {
        "id": "E2buyVqBR5OV"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "Now, we're curious if we can pick out years that had droughts or lots of rainfall. Let's make a lineplot of year and monthly moisture. We see a massive dip in 2012 indicating there was probably a drought in some places. Upon doing further research, there was a drought in 2012! To learn more about the Drought of 2012, click the link [here](https://www.weather.gov/iwx/2012_drought). Let's see if there is any obvious impact of state on monthly moisture as well. This graph is really hard to read due to do how many states there are but it doesn't seem to have too much of an impact on monthly moisture trends, beyond the fact that certain states get less rainfall than others."
      ],
      "metadata": {
        "id": "hWQymqeTlhOS"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "sns.lineplot(x='YEAR', y='month_moisture',\n",
        "             data=df)"
      ],
      "metadata": {
        "id": "SC10trNAlhnA"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "sns.lineplot(x='YEAR', y='month_moisture', hue= 'State',\n",
        "             data=df)"
      ],
      "metadata": {
        "id": "l86Lmy5qliiX"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "The next thing we want to see is if there is a relationship between the amount of monthly moisture and the source of a municipalities drinking water. Maybe if a place is dry they'll be more likely to use ground water or purchased water?"
      ],
      "metadata": {
        "id": "nY5IEo5IntxX"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "sns.violinplot(x='PRIMARY_SOURCE_CODE', y='month_moisture',\n",
        "             data=df)"
      ],
      "metadata": {
        "id": "aEZ8PGd1ns4F"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "Interesting, again we don't see tons of variance. The only point of interest is that areas that have purchased groundwater are more likely to have a higher moisture.\n",
        "\n",
        "The last thing we'll look at is, if the 2012 drought influenced how much money municipalities spent on water. To do this, we'll look at the cost of water utilities before 2012, and during 2012."
      ],
      "metadata": {
        "id": "nGzLBBfzqVNK"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df_pre2012= df.loc[df['YEAR'] <= 2012]\n",
        "\n",
        "sns.lineplot(x= \"YEAR\", y= \"Water_Util_Total_Exp\", data= df_pre2012)"
      ],
      "metadata": {
        "id": "sfF7XYEBXxxh"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "So hear we noticed that the amount spent on water utilities by municipalities does increase from 1997 to 2007 but plateaus from 2007 to 2012. This doesn't support our suspicion that the 2012 drough would cause municipalities to spend more on water."
      ],
      "metadata": {
        "id": "Y_YDJneQXX69"
      }
    }
  ]
}
