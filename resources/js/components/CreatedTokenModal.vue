<template>
  <Modal :show="show" size="4xl">
    <div
      class="mx-auto bg-white dark:bg-gray-800 rounded-lg shadow-lg overflow-hidden"
    >
      <slot>
        <ModalHeader>{{ __("Created Token") }}</ModalHeader>
        <ModalContent>
          <div class="flex flex-col">
            <div class="flex flex-col space-y-2">
              <div
                class="text-lg bg-gray-100 dark:bg-gray-700 hover:bg-gray-50 dark:hover:bg-gray-900 hover:text-gray-500 active:text-gray-600 rounded p-4"
              >
                <button
                  v-tooltip="__('Copy to clipboard')"
                  type="button"
                  class="flex items-center justify-between w-full text-gray-500 dark:text-gray-400 rounded-lg px-1 -mx-1"
                  @click.prevent.stop="handleCopy(newToken)"
                >
                  <span ref="theFieldValue">
                    {{ newToken }}
                  </span>

                  <Icon
                    v-if="copied"
                    class="text-green-500"
                    :solid="true"
                    type="check-circle"
                    width="14"
                  />
                  <Icon
                    v-if="!copied"
                    class="text-gray-400 dark:text-gray-500"
                    :solid="true"
                    type="clipboard"
                    width="14"
                  />
                </button>
              </div>
              <HelpText class="mt-2 help-text-error">
                {{
                  __(
                    "Make sure to copy your new personal access token now. You won't be able to see it again!"
                  )
                }}
              </HelpText>
            </div>
          </div>
        </ModalContent>
      </slot>

      <ModalFooter>
        <div class="ml-auto">
          <DefaultButton type="button" @click.prevent="handleConfirmed">
            {{ __("Confirm") }}
          </DefaultButton>
        </div>
      </ModalFooter>
    </div>
  </Modal>
</template>

<script setup>
import { ref } from "vue";

const copied = ref(false);

defineProps({
  newToken: {
    required: true,
    type: String,
  },
  show: { type: Boolean, default: false },
});

const emit = defineEmits(["confirmed"]);

const handleConfirmed = () => {
  emit("confirmed");
};

const handleCopy = (value) => {
  navigator.clipboard.writeText(value).then(
    function () {
      copied.value = true;
      setTimeout(() => (copied.value = !copied.value), 2000);
    },
    function () {
      copied.value = false;
    }
  );
};
</script>
