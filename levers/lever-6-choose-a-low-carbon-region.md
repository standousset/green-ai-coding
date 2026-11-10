[← Lever 5: Make fewer round trips](lever-5-make-fewer-round-trips.md) &emsp; [All levers](../README.md)

# Lever 6: Choose a low-carbon region

> [!IMPORTANT]
> When you can choose where your requests run, choose a region with low-carbon electricity, and check where they really run.

**Why it matters:** The CO₂ that a kWh emits depends on where the electricity is
made. In 2025, it was 28 gCO₂ per kWh in Norway and 670 in India, a gap of more
than 20 times
([Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)).
This lever applies only if you can choose where requests run. Most ways to use
Claude give no such setting in the docs (see 6.2).

| Item | Role |
|---|---|
| [6.1 Know that the same energy emits very different amounts of CO₂](#61-know-that-the-same-energy-emits-very-different-amounts-of-co₂) | Learn the gap |
| [6.2 Check whether you can choose a region](#62-check-whether-you-can-choose-a-region) | Check your options |
| [6.3 Find the carbon data of a region](#63-find-the-carbon-data-of-a-region) | Find the data |
| [6.4 Choose a region with low-carbon electricity when you can](#64-choose-a-region-with-low-carbon-electricity-when-you-can) | Set the region |
| [6.5 Check where your requests really run](#65-check-where-your-requests-really-run) | Check the result |

## 6.1 Know that the same energy emits very different amounts of CO₂

**What:** The CO₂ of a request depends on the electricity of the data centre that
serves it, and so on its country.

**Why:** Carbon intensity of electricity in 2025
([Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)).
The data comes from Ember and the Energy Institute:

| Country or region | gCO₂ per kWh |
|---|---:|
| Norway | 28 |
| Sweden | 35 |
| France | 41 |
| European Union | 210 |
| United States | 384 |
| World | 458 |
| China | 525 |
| Poland | 589 |
| India | 670 |

From Norway to India, the gap is about 24 times.

**How:** Use the table to see the size of the gap. Items 6.3 and 6.4 show how to
find the data of a region and how to use it.

**Where:**
- All ways: the table above. The country of the data centre that serves a
  request applies to it.

**Watch out:** These are yearly averages for a whole country. A live value changes
with the hour, and a data centre can buy clean power
([Electricity Maps](https://www.electricitymaps.com/)).

## 6.2 Check whether you can choose a region

**What:** Find out what your way of using Claude lets you choose.

**Why:** You can act on 6.1 only if you can choose where requests run. The docs
give a setting for some ways and none for the others.

**How:**

| How you use Claude | What you can choose |
|---|---|
| Amazon Bedrock | The AWS Region, with `AWS_REGION`. If you set nothing, Claude Code uses `us-east-1` ([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock#3-configure-claude-code)). |
| Google Cloud's Agent Platform | `CLOUD_ML_REGION`: `global`, a multi-region location such as `eu` or `us`, or one region. If you set nothing, Claude Code uses `us-east5` ([Claude Code docs](https://code.claude.com/docs/en/google-vertex-ai#region-configuration)). |
| Microsoft Foundry | The docs give no region setting ([Claude Code docs](https://code.claude.com/docs/en/microsoft-foundry)). |
| Claude API | `inference_geo`: `us` or `global`. This is a choice of geography, not of a low-carbon region ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/data-residency)). |
| Claude subscription | The docs that I read give no region setting. |

**Where:**
- Terminal: on Bedrock, `/status` shows the region that Claude Code resolved
  ([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock#3-configure-claude-code)).
- Desktop app, Code tab and Chat: the docs that I read give no region setting.

**Watch out:** If you run your own models, you choose the region of your own cloud
account.

## 6.3 Find the carbon data of a region

**What:** Look up the carbon data of a region before you choose it.

**Why:** Two regions of the same provider can differ a lot. In the 2024 data of
Google Cloud, the grid of `europe-north2` (Stockholm) emits 3 gCO₂eq per kWh. The
grid of `asia-south1` (Mumbai) emits 679. In the United States, `us-west1`
(Oregon) emits 79, and `us-east1` (South Carolina) emits 576
([Google Cloud region data](https://github.com/GoogleCloudPlatform/region-carbon-info)).

**How:**
- Google Cloud publishes, for each of its regions, the share of carbon-free energy
  and the carbon intensity of the grid. The data is on the
  [carbon page](https://cloud.google.com/sustainability/region-carbon), in a
  [GitHub repository](https://github.com/GoogleCloudPlatform/region-carbon-info)
  and in a BigQuery public dataset. Google calls a region low-carbon when
  carbon-free energy is at least 75%, or when the grid emits at most 200 gCO₂eq
  per kWh.
- For other providers, use the country data of
  [Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)
  (see 6.1). [Electricity Maps](https://www.electricitymaps.com/) shows live data
  for more than 130 zones on an interactive map. Its API has a 14-day trial, and
  then needs a paid plan.

**Where:**
- All ways: these are web pages, and not settings of Claude.

**Watch out:** Google says that its data "has not been assured". The data comes
from Electricity Maps. The share of carbon-free energy and the grid intensity are
two different values. In `us-central1` (Iowa), they are 87% and 413 gCO₂eq per
kWh. This guide found no per-region carbon data from AWS or Microsoft.

## 6.4 Choose a region with low-carbon electricity when you can

**What:** If you use Claude through a cloud provider, set the region to one with
low-carbon electricity.

**Why:** The same energy emits less CO₂ in a region with a clean grid (see 6.1).
Google tells you to "pick a cleaner region for your new applications"
([Google Cloud](https://cloud.google.com/sustainability/region-carbon)).

**How:**
1. Find a candidate region in 6.3.
2. Check what else matters: latency, price and your rules on data location
   ([Google Cloud](https://cloud.google.com/sustainability/region-carbon)). Check
   that the region has your Claude model, and that you have quota there.
3. Set the region. On Google Cloud, run `export CLOUD_ML_REGION=europe-north1`, for
   example. On Bedrock, set `AWS_REGION` to the region that you chose.
4. On Google Cloud, some models are offered only on `global` or multi-region
   locations, so you may need `VERTEX_REGION_CLAUDE_*` for one model
   ([Claude Code docs](https://code.claude.com/docs/en/google-vertex-ai#region-configuration)).
   On Bedrock, list the profiles of a region with `aws bedrock list-inference-profiles --region your-region`
   ([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock)).

**Where:**
- Terminal: the variables above.
- Desktop app, Code tab and Chat: the docs that I read give no region setting.

**Watch out:** `CLOUD_ML_REGION=global` does not choose a region: the docs
suggest it for better availability. If you use a geography instead of a region,
read 6.5.

## 6.5 Check where your requests really run

**What:** On Bedrock, check which inference profile Claude Code uses, because it
can send your requests to other AWS Regions.

**Why:** Cross-region inference "routes your requests by using compute across
different AWS Regions". A geographic profile stays inside its geography. For the
US profile, the AWS example lists `us-east-1`, `us-east-2` and `us-west-2`. A
global profile can use any supported commercial Region
([AWS docs](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)).
Claude Code picks the profile from your region: `us-` gives `us.`, `eu-` gives
`eu.`, `ap-` gives `apac.`, and every other region gives `global.`. The docs call
this "a preference, not a guarantee"
([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock#cross-region-inference-profile-prefixes)).

**How:**
- To pick the profile, set `ANTHROPIC_BEDROCK_REGION_PREFIX` to `us`, `eu`,
  `apac`, `jp`, `au` or `global`. If your account has no profile with that prefix,
  requests fail with a 400 error.
- To see where a request ran, look at the `additionalEventData.inferenceRegion`
  field in CloudTrail
  ([AWS docs](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)).

**Where:**
- Terminal: the variable above.

**Watch out:** The AWS docs that I read do not say if you can force one single
Region. They also do not list every destination Region of each geography. A global
profile costs about 10% less than a geographic one, but it routes worldwide
([AWS docs](https://docs.aws.amazon.com/bedrock/latest/userguide/geographic-cross-region-inference.html)).

---

[← Lever 5: Make fewer round trips](lever-5-make-fewer-round-trips.md) &emsp; [All levers](../README.md)
