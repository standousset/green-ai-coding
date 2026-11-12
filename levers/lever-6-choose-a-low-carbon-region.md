[← Lever 5: Make fewer round trips](lever-5-make-fewer-round-trips.md) &emsp; [All levers](../README.md)

# Lever 6: Choose a low-carbon region

*Reading time: about 9 minutes.*

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
| [6.1 Know that the same energy emits very different amounts of CO₂](#61-know-that-the-same-energy-emits-very-different-amounts-of-co) | Learn the gap |
| [6.2 Check whether you can choose a region](#62-check-whether-you-can-choose-a-region) | Check your options |
| [6.3 Find the carbon data of a region](#63-find-the-carbon-data-of-a-region) | Find the data |
| [6.4 Choose a region with low-carbon electricity when you can](#64-choose-a-region-with-low-carbon-electricity-when-you-can) | Set the region |
| [6.5 Check where your requests really run](#65-check-where-your-requests-really-run) | Check the result |
| [6.6 Trust the grid of the region more than a claim based on contracts](#66-trust-the-grid-of-the-region-more-than-a-claim-based-on-contracts) | Read the figures |

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
- All ways: the table above. The grid at the place of the data centre that
  serves a request applies to it.

**Watch out:**
- These are yearly averages for a whole country. A live value changes with the
  hour ([Electricity Maps](https://www.electricitymaps.com/)).
- A country average can be far from the grid of one data centre. In the Google
  data of 2025 (see 6.3), the grid of `us-west1` (Oregon) emits 98 gCO₂eq per kWh.
  The grid of `us-east1` (South Carolina) emits 579. The average of the United
  States is 384. In Montréal the grid emits 11, and the average of Canada is 191.
  The two sources differ, so compare the size of the gaps and not the exact
  values.
- A data centre that is connected to the grid uses the mix of its local grid. A
  figure that is lower than the grid needs a power plant on site, a direct line to
  a power plant, or contracts. A figure that rests on contracts is market-based
  (see 6.6).

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
- Desktop app, Code tab: the docs that I read give no region setting.

**Watch out:** If you run your own models, you choose the region of your own cloud
account.

## 6.3 Find the carbon data of a region

**What:** Look up the carbon data of a region before you choose it.

**Why:** Two regions of the same provider can differ a lot. In the 2025 data of
Google Cloud, the grid of `europe-north2` (Stockholm) emits 19 gCO₂eq per kWh. The
grid of `asia-south1` (Mumbai) emits 598. In the United States, `us-west1`
(Oregon) emits 98, and `us-east1` (South Carolina) emits 579
([Google Cloud region data](https://github.com/GoogleCloudPlatform/region-carbon-info)).

**How:**
- Google Cloud publishes, for each of its regions, the share of carbon-free energy
  and the carbon intensity of the grid. The data is on the
  [carbon page](https://cloud.google.com/sustainability/region-carbon), in a
  [GitHub repository](https://github.com/GoogleCloudPlatform/region-carbon-info)
  and in a BigQuery public dataset. Google calls a region low-carbon when
  carbon-free energy is at least 75%. If that value is not available, the limit
  is a grid of at most 200 gCO₂eq per kWh.
- For other providers, use the country data of
  [Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)
  (see 6.1). [Electricity Maps](https://www.electricitymaps.com/) shows live data
  on an interactive map. Its site offers a trial of its API: check the current
  terms.

**Where:**
- All ways: these are web pages, and not settings of Claude.

**Watch out:** The data comes from Electricity Maps. The Google page says that
Electricity Maps has an independent Letter of Assurance for its data pipeline and
calculation models. The share of carbon-free energy and the grid intensity are
two different values. In `us-central1` (Iowa), they are 88% and 432 gCO₂eq per
kWh. The share takes into account the renewable investments of Google, so it
does not show which electricity the data centre uses (see 6.6). To compare
regions, rank them by the grid value. This guide found no table of grid carbon
intensity per region from AWS or Microsoft.

## 6.4 Choose a region with low-carbon electricity when you can

**What:** If you use Claude through a cloud provider, set the region to one with
low-carbon electricity.

**Why:** The same energy emits less CO₂ in a region with a clean grid (see 6.1).
Google tells you to "pick a cleaner region for your new applications"
([Google Cloud](https://cloud.google.com/sustainability/region-carbon)).

**How:**
1. Find a candidate region in 6.3. Rank the regions by the grid carbon intensity,
   and not by the carbon-free share (see 6.6).
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
- Desktop app, Code tab: the docs that I read give no region setting.

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
Claude Code picks the profile from your region. A region that starts with `us-`
gives `us.`, `us-gov-` gives `us-gov.`, `eu-` gives `eu.` and `ap-` gives `apac.`.
Every other region gives `global.`. The docs call this "a preference, not a
guarantee"
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
([AWS docs](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)).

## 6.6 Trust the grid of the region more than a claim based on contracts

**What:** Choose a region by its location-based figure. When a provider says that
its data centres in a country are cleaner than the grid, check how the figure is
made.

**Why:**
- A location-based figure uses the emissions of the grid where the energy is
  used. A market-based figure also counts the clean power that a company buys.
  The paper of Google on Gemini defines both, and it uses the market-based one
  ([Google paper](https://arxiv.org/abs/2508.15734)).
- A market-based figure can be far from the electricity that the data centre
  uses. In the Google data of 2025, `us-central1` (Iowa) has a carbon-free share
  of 88% and a grid of 432 gCO₂eq per kWh. The README of the data says that the
  share takes into account the renewable investments of Google in that location
  ([Google region data](https://github.com/GoogleCloudPlatform/region-carbon-info)).
- Ireland is an example. Its grid emitted 257 gCO₂ per kWh in 2025
  ([Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)).
  Data centres used 23% of its metered electricity
  ([Data Centre Review](https://datacentrereview.com/2026/07/data-centres-consumed-23-of-irelands-electricity-in-2025/),
  from the Central Statistics Office). A data centre that is connected to this
  grid uses this mix. New rules expect new data centres to provide matching
  on-site generation. They also expect at least 80% of the annual demand to come
  from new renewable projects (same article). This guide found no published
  figure for the effect of these rules.

**How:** Look at what the provider publishes, as in the table (checked on
12 November 2026). Use a location-based figure when there is one.

| Provider | What it publishes |
|---|---|
| Google Cloud | A grid carbon intensity and a carbon-free share for each region ([Google Cloud](https://cloud.google.com/sustainability/region-carbon)). The grid data comes from Electricity Maps. For a median Gemini Apps text prompt, Google gives 0.24 Wh and 0.03 gCO₂e. The emissions figure is market-based, and the paper says that the purchase of clean energy is part of the fall of the figure over one year ([Google paper](https://arxiv.org/abs/2508.15734)). |
| AWS | Since June 2025, the Customer Carbon Footprint Tool shows location-based and market-based emissions for your own usage ([AWS](https://aws.amazon.com/about-aws/whats-new/2025/06/customer-carbon-footprint-tool-location-based-emissions)). This guide found no table of the carbon intensity of each region. |
| Microsoft | The Emissions Impact Dashboard shows your emissions by datacenter region ([Microsoft](https://azure.microsoft.com/en-us/blog/empowering-cloud-sustainability-with-the-microsoft-emissions-impact-dashboard/)). This guide found no table of location-based intensity for each region. |
| Mistral | A life-cycle analysis of Mistral Large 2, made with Carbone 4 and ADEME. It uses the location-based approach and counts the manufacturing of the servers. A response of 400 tokens in Le Chat has 1.14 gCO₂e ([Mistral](https://mistral.ai/news/our-contribution-to-a-global-environmental-standard-for-ai)). The text that I read does not say where the data centres are. |

**Where:**
- All ways: these are published figures, not settings.

**Watch out:**
- This guide found no provider that publishes a location-based figure for each
  data centre. The Google column is for a region, and it comes from the grid of
  its zone.
- The two figures for one prompt count different things and use different
  methods. Do not compare them.
- This guide did not look at other providers, such as OpenAI or GitHub Copilot.

---

[← Lever 5: Make fewer round trips](lever-5-make-fewer-round-trips.md) &emsp; [All levers](../README.md)
