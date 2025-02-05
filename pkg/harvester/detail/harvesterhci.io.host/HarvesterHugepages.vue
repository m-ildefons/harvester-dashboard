<script>
import LabelValue from '@shell/components/LabelValue';
import { HCI } from '../../types';
import { DOC_LINKS } from '../../config/doc-links.js';

export default {
  name:       'HarvesterHugepages',
  components: { LabelValue },

  props: {
    node: {
      type:     Object,
      required: true,
    },
  },

  computed: {
    docsTransparentHugepagesLink() {
      return DOC_LINKS.TRANSPARENT_HUGEPAGES;
    },
    docsHugepagesLink() {
      return DOC_LINKS.HUGETLBFS;
    },
  },

  async fetch() {
    const inStore = this.$store.getters['currentProduct'].inStore;

    const hash = await this.$store.dispatch(`${ inStore }/findAll`, { type: HCI.HUGEPAGES });

    this.hugepages = hash.find((node) => {
      return node.id === this.node.id;
    });
  },

  data() {
    return { hugepages: {} };
  },
};
</script>

<template>
  <div>
    <template v-if="hugepages.status">
      <h2>{{ t('harvester.host.hugepages.meminfo') }}</h2>

      <div class="row mb-20">
        <div class="col span-6">
          <LabelValue :name="t('harvester.host.hugepages.status.anon')" :value="hugepages.status.meminfo.anonHugePages" />
        </div>
        <div class="col span-6">
          <LabelValue :name="t('harvester.host.hugepages.status.size')" :value="hugepages.status.meminfo.hugepageSize" />
        </div>
      </div>

      <div class="row mb-20">
        <div class="col span-3">
          <LabelValue :name="t('harvester.host.hugepages.status.total')" :value="hugepages.status.meminfo.hugePagesTotal" />
        </div>
        <div class="col span-3">
          <LabelValue :name="t('harvester.host.hugepages.status.free')" :value="hugepages.status.meminfo.hugePagesFree" />
        </div>
        <div class="col span-3">
          <LabelValue :name="t('harvester.host.hugepages.status.rsvd')" :value="hugepages.status.meminfo.hugePagesRsvd" />
        </div>
        <div class="col span-3">
          <LabelValue :name="t('harvester.host.hugepages.status.surp')" :value="hugepages.status.meminfo.hugePagesSurp" />
        </div>
      </div>

      <div>
        <hr class="divider" />
        <h3>
          <t k="harvester.host.hugepages.transparent.title" :raw="true" :url="docsTransparentHugepagesLink" />
        </h3>

        <div class="row mb-20">
          <div class="col span-4">
            <LabelValue :name="t('harvester.host.hugepages.transparent.enabled')" :value="hugepages.spec.transparent.enabled" />
          </div>
          <div class="col span-4">
            <LabelValue :name="t('harvester.host.hugepages.transparent.shmemEnabled')" :value="hugepages.spec.transparent.shmemEnabled" />
          </div>
          <div class="col span-4">
            <LabelValue :name="t('harvester.host.hugepages.transparent.defrag')" :value="hugepages.spec.transparent.defrag" />
          </div>
        </div>
      </div>

      <div>
        <hr class="divider" />
        <h3>
          <t k="harvester.host.hugepages.hugetlbfs.title" :raw="true" :url="docsHugepagesLink" />
        </h3>

        <ul>
          <template v-for="item in hugepages.status.hugetlbfs">
            <div :key="item.pagesize" class="row mb-20">
              <div class="col span-2">
                <LabelValue :name="t('harvester.host.hugepages.hugetlbfs.size')" :value="item.pagesize" />
              </div>
              <div class="col span-2">
                <LabelValue :name="t('harvester.host.hugepages.hugetlbfs.mountpoint')" :value="item.mountpoint" />
              </div>
              <div class="col span-2">
                <LabelValue :name="t('harvester.host.hugepages.hugetlbfs.free')" :value="item.free" />
              </div>
              <div class="col span-2">
                <LabelValue :name="t('harvester.host.hugepages.hugetlbfs.rsvd')" :value="item.reserved" />
              </div>
              <div class="col span-2">
                <LabelValue :name="t('harvester.host.hugepages.hugetlbfs.surplus')" :value="item.surplus" />
              </div>
            </div>
          </template>
        </ul>
      </div>
    </template>
  </div>
</template>
