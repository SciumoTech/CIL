WARNING
=======

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

The authors claim absolutely no responsibility or liability for the use or
misuse of this program by any teams or individuals related to the DARPA
Spectrum Collaboration Challenge. This tool is not endorsed or supported by
DARPA.

Scoring Tool
============

This python package contains scoring tools for analyzing DRC traffic logs
and mandated outcome specification files generated by the SC2 colosseum.
It has a C++ component to aggregate data from each DRC file into measurement
period statistics, and python components to read the aggregated data and
generate scoring information. Each component can be used as a library or a
standalone program as follows:

- [scoring_parser](scoringparser/src) reads one or more DRC files and produces
  aggregated measurement period statistics including counts of sent, received,
  duplicate, and late packet for each flow.
- [mandate_reader.py](scoringtool/mandate_reader.py) reads mandate files
  for each node from a mandated outcomes directory, and parses mandate
  information for each flow.
- [scoring_reader.py](scoringtool/scoring_reader.py) wraps around the C++
  scoring parser to read aggregated measurement period statistics for each
  flow from a traffic_logs directory.
- [scoring_checker.py](scoringtool/scoring_checker.py) calculates scoring
  data per team and per match from a common match log file directory and a
  mandated outcomes directory. Reports can be generated in a selection of
  formats.

You can install this package with `pip install -e .` and then you can access
all commands with the `scoringtool` program. Try running  `scoringtool --help`
for help. 

## Common Usage

To output stage scores and overall match score:

```bash
scoringtool scoring-checker --mandates <mandates-dir> --common-logs <common-logs-dir> --environment <environment-dir>
```

To additionally output scores per measurement period:

```bash
scoringtool scoring-checker --mandates <mandates-dir> --common-logs <common-logs-dir> --environment <environment-dir> --mp_scores
```

To generate a report based on the [DARPA Phase 3 Scores JSON format](https://sc2colosseum.freshdesk.com/support/solutions/articles/22000239290-phase-3-scores-json-file-format)
and the [DARPA Phase 3 Goals JSON format](https://sc2colosseum.freshdesk.com/support/solutions/articles/22000239289-phase-3-goals-json-file-format):

```bash
scoringtool scoring-checker --mandates <mandates-dir> --common-logs <common-logs-dir> --environment <environment-dir> --end-margin 15 --output-format darpa
scoringtool scoring-checker --mandates <mandates-dir> --common-logs <common-logs-dir> --environment <environment-dir> --end-margin 15 --output-format darpa_goals
```

To plot scoring data in Gnuplot:

```bash
scoringtool scoring-checker --mandates <mandates-dir> --common-logs <common-logs-dir> --environment <environment-dir> --output-format gnuplot | gnuplot --persist
```

Where `<mandates-dir>` is a path similar to `[...]/scenarios/7012/Mandated_Outcomes`,
`<environment-dir>` is a path similar to `[...]/scenarios/7012/Environment`,
and `<common-logs-dir>` is a path similar to `[...]/scrimmage1_data/common/MATCH-001-RES-017926`.
