[← Lever 5: Make fewer round trips](lever-5-make-fewer-round-trips.md) &emsp; [All levers](../README.md)

# Lever 6: Choose a low-carbon region

*Reading time: about 7 minutes.*

> [!IMPORTANT]
> When you can choose where your requests run, choose a region with low-carbon electricity, and check where they really run.

**Why it matters:** The CO₂ that a kWh emits depends on where the electricity is
made. In 2025, it was 28 gCO₂ per kWh in Norway and 670 in India, a gap of about 24
times
([Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)).
So the CO₂ of a request depends on the electricity of the data centre that serves
it.

This lever only applies if you can choose where your requests run. In the docs,
only Amazon Bedrock and Google Cloud's Agent Platform give a region setting. A
Claude subscription gives none, and the Claude API gives a choice of geography
only (see 6.1). This lever doesn't lower the energy of a request. It lowers the
CO₂ of the same energy.

The table shows the carbon intensity of electricity in 2025. The data comes from
Ember and the Energy Institute
([Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)):

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

These figures have two limits:
- These are yearly averages for a whole country. A live value changes with the
  hour ([Electricity Maps](https://www.electricitymaps.com/)).
- A country average can be far from the grid of one data centre. In the Google
  data of 2025 (see 6.2), the grid of `us-west1` (Oregon) emits 98 gCO₂eq per kWh.
  The grid of `us-east1` (South Carolina) emits 579. The average of the United
  States is 384. In Montréal the grid emits 11, and the average of Canada is 191.
  The two sources differ, so compare the size of the gaps and not the exact
  values.

| Item | Role |
|---|---|
| [6.1 Check whether you can choose a region](#61-check-whether-you-can-choose-a-region) | Check your options |
| [6.2 Find the carbon data of a region and rank regions by the grid](#62-find-the-carbon-data-of-a-region-and-rank-regions-by-the-grid) | Find and read the data |
| [6.3 Choose a region with low-carbon electricity when you can](#63-choose-a-region-with-low-carbon-electricity-when-you-can) | Set the region |
| [6.4 Check where your requests really run](#64-check-where-your-requests-really-run) | Check the result |

## 6.1 Check whether you can choose a region

**What:** Find out what your way of using Claude lets you choose.

**Why:** You can only act on the gap above if you can choose where requests run.
For some ways of using Claude the docs give a setting, and for the others they
give none.

**How:**

| How you use Claude | What you can choose |
|---|---|
| Amazon Bedrock | The AWS Region, with `AWS_REGION`. If you set nothing, Claude Code uses `us-east-1` ([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock#3-configure-claude-code)). |
| Google Cloud's Agent Platform | `CLOUD_ML_REGION`: `global`, a multi-region location such as `eu` or `us`, or one region. If you set nothing, Claude Code uses `us-east5` ([Claude Code docs](https://code.claude.com/docs/en/google-vertex-ai#region-configuration)). |
| Microsoft Foundry | The docs give no region setting ([Claude Code docs](https://code.claude.com/docs/en/microsoft-foundry)). |
| Claude API | `inference_geo`: `us` or `global`. This is a choice of geography, not of a low-carbon region ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/data-residency)). |
| Claude subscription | The docs give no region setting. |

**Where:**
- Terminal: on Bedrock, `/status` shows the region that Claude Code resolved
  ([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock#3-configure-claude-code)).
- Desktop app, Code tab: the docs give no region setting.

**Watch out:**
- On Bedrock, a region setting doesn't always keep your requests in that region
  (see 6.4).
- If you run your own models, you choose the region of your own cloud account.

## 6.2 Find the carbon data of a region and rank regions by the grid

**What:** Look up the carbon data of each region that you can use. Rank the regions
by the carbon intensity of their grid, and not by a carbon-free share or a claim
about clean power contracts.

**Why:**
- Two regions of the same provider can differ a lot. In the 2025 data of Google
  Cloud, the grid of `europe-north2` (Stockholm) emits 19 gCO₂eq per kWh. The grid
  of `asia-south1` (Mumbai) emits 598
  ([Google Cloud region data](https://github.com/GoogleCloudPlatform/region-carbon-info)).
- A location-based figure uses the emissions of the grid where the energy is
  used. A market-based figure also counts the clean power that a company buys.
  The paper of Google on Gemini defines both, and it uses the market-based one
  ([Google paper](https://arxiv.org/abs/2508.15734)).
- A market-based figure can be far from the electricity that the data centre
  uses. A data centre that is connected to the grid uses the mix of its local grid.
  In the Google data of 2025, `us-central1` (Iowa) has a carbon-free share of 88%
  and a grid of 432 gCO₂eq per kWh. The README of the data says that the share
  takes into account the renewable investments of Google in that location
  ([Google Cloud region data](https://github.com/GoogleCloudPlatform/region-carbon-info)).

**How:**
- Google Cloud publishes, for each of its regions, the share of carbon-free energy
  and the carbon intensity of the grid. The data is on the
  [carbon page](https://cloud.google.com/sustainability/region-carbon), in a
  [GitHub repository](https://github.com/GoogleCloudPlatform/region-carbon-info)
  and in a BigQuery public dataset. Rank by the grid value. Google calls a region
  low-carbon when carbon-free energy is at least 75%. If that value isn't
  available, the limit is a grid of at most 200 gCO₂eq per kWh.
- For other providers, use the country data of
  [Our World in Data](https://ourworldindata.org/grapher/carbon-intensity-electricity)
  (see the table above). [Electricity Maps](https://www.electricitymaps.com/) shows
  live data on an interactive map. Its site offers a trial of its API: check the
  current terms.
- AWS and Microsoft publish tools for your own usage, not a table for each region
  (checked on 12 November 2026). Since June 2025, the AWS Customer Carbon
  Footprint Tool shows location-based and market-based emissions
  ([AWS](https://aws.amazon.com/about-aws/whats-new/2025/06/customer-carbon-footprint-tool-location-based-emissions)).
  The Microsoft Emissions Impact Dashboard shows your emissions by datacenter
  region
  ([Microsoft](https://azure.microsoft.com/en-us/blog/empowering-cloud-sustainability-with-the-microsoft-emissions-impact-dashboard/)).
  Use a location-based figure when there is one.

**Watch out:**
- The grid data of Google comes from Electricity Maps. The Google page says that
  Electricity Maps has an independent Letter of Assurance for its data pipeline
  and its calculation models.
- None of these providers publishes a location-based figure for each data centre.
  The Google figure is for a region, and it comes from the grid of its zone.
- Providers also give figures for one prompt. For example, Google gives a
  market-based figure for a Gemini prompt. These figures count different things
  and use different methods. Don't compare them, and don't use them to choose a
  region.
- Other providers, such as OpenAI or GitHub Copilot, aren't covered.

## 6.3 Choose a region with low-carbon electricity when you can

**What:** If you use Claude through a cloud provider, set the region to one with
low-carbon electricity.

**Why:** The same energy emits less CO₂ in a region with a clean grid (see the table
above). Google tells you to "pick a cleaner region for your new applications"
([Google Cloud](https://cloud.google.com/sustainability/region-carbon)).

Example, from the Google data of 2025. The default region of Claude Code on Google
Cloud, `us-east5` (Columbus), has a grid of 343 gCO₂eq per kWh. The grid of
`us-west1` (Oregon) emits 98, about 3.5 times less. The grid of `europe-north1`
(Finland) emits 30, more than 11 times less
([Google Cloud region data](https://github.com/GoogleCloudPlatform/region-carbon-info)).
This is a gap in the grid, not a measure of one request.

**How:**
1. Find a candidate region in 6.2. Rank the regions by the grid carbon intensity.
2. Check what else matters: latency, price and your rules on data location
   ([Google Cloud](https://cloud.google.com/sustainability/region-carbon)). Check
   that the region has your Claude model, and that you have quota there.
3. Set the region. On Google Cloud, run `export CLOUD_ML_REGION=europe-north1`, for
   example. On Bedrock, set `AWS_REGION` to the region that you chose.
4. On Google Cloud, some models are offered only on `global` or multi-region
   locations, so you may need `VERTEX_REGION_CLAUDE_*` for one model
   ([Claude Code docs](https://code.claude.com/docs/en/google-vertex-ai#region-configuration)).
   On Bedrock, list the profiles of a region with
   `aws bedrock list-inference-profiles --region your-region`
   ([Claude Code docs](https://code.claude.com/docs/en/amazon-bedrock)).

**Watch out:** `CLOUD_ML_REGION=global` doesn't choose a region: the docs
suggest it for better availability. On Bedrock, if you use a geography instead of
a region, read 6.4.

## 6.4 Check where your requests really run

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

**Watch out:** The AWS docs don't say whether you can force one single Region.
They also don't list every destination Region of each geography. A global
profile costs about 10% less than a geographic one, but it
routes worldwide
([AWS docs](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)).

---

[← Lever 5: Make fewer round trips](lever-5-make-fewer-round-trips.md) &emsp; [All levers](../README.md)
