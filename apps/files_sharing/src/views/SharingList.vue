<!--
  - SPDX-FileCopyrightText: 2019 Nextcloud GmbH and Nextcloud contributors
  - SPDX-License-Identifier: AGPL-3.0-or-later
-->

<template>
	<ul class="sharing-sharee-list" :aria-label="t('files_sharing', 'Shares')">
		<SharingEntry v-for="share in shares"
			:key="share.id"
			:file-info="fileInfo"
			:share="share"
			:is-unique="isUnique(share)"
			@open-sharing-details="openSharingDetails(share)" />
	</ul>
</template>

<script>
import { t } from '@nextcloud/l10n'
import SharingEntry from '../components/SharingEntry.vue'
import ShareTypes from '../mixins/ShareTypes.js'
import ShareDetails from '../mixins/ShareDetails.js'

export default {
	name: 'SharingList',

	components: {
		SharingEntry,
	},

	mixins: [ShareTypes, ShareDetails],

	props: {
		fileInfo: {
			type: Object,
			default: () => { },
			required: true,
		},
		shares: {
			type: Array,
			default: () => [],
			required: true,
		},
	},

	setup() {
		return {
			t,
		}
	},
	computed: {
		hasShares() {
			return this.shares.length === 0
		},
		isUnique() {
			return (share) => {
				return [...this.shares].filter((item) => {
					return share.type === this.SHARE_TYPES.SHARE_TYPE_USER && share.shareWithDisplayName === item.shareWithDisplayName
				}).length <= 1
			}
		},
	},
}
</script>
