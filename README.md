# WhiteSource-EUA-scan

name: WhiteSource EUA Scan
on:
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest
    env:
       APP_PATH: <VALUE>
    steps:
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}
        
      - name: WhiteSource CI integration
        uses: Idancc/WhiteSource-CI-Integration@<CHOOSE VERSION>
        env:
          WHITESOURCE_PRODUCT_NAME: ${{ secrets.WHITESOURCE_PRODUCT_NAME }}
          WHITESOURCE_PROJECT_NAME: ${{ github.event.repository.name }}
          WHITESOURCE_GH_PAT: ${{ secrets.WHITESOURCE_GH_PAT }}
          WHITESOURCE_CONFIG_REPO: ${{ secrets.WHITESOURCE_CONFIG_REPO }}
          WHITESOURCE_NPM_TOKEN: ${{ secrets.WHITESOURCE_NPM_TOKEN }}
          WHITESOURCE_API_KEY: ${{ secrets.WHITESOURCE_API_KEY }}
          WHITESOURCE_USER_KEY: ${{ secrets.WHITESOURCE_USER_KEY }}
        
      - name: policy Rejection Summary
        if: ${{ always() }}
        run: cat ./whitesource/policyRejectionSummary.json
